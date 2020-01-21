<template>
<div id="explorecard">
		<div class="row mt-5-1">
			<div class="col-md-6 col-10 mx-auto text-center banner-tx mt-banner">
				Welcome to Quras’s Non fungible token (NFT) explorer and minter. 
			</div>
		</div>

		<div class="row mt-5-1">
			<div class="col-10 col-md-6 mx-auto description-tx">
				This POC is to help developers get started with NFT’s in the NEO Smart Economy.
				<a href="#">Learn More</a>
			</div>
		</div>

		<div class="row mt-5-1">
			<div class="col-10 col-md-6 mx-auto" style="width:100%">
				<div class="input-group mb-1">
					<input class="form-control" v-model="search_value" v-bind:class= "{'is-valid': valid_input == true, 'is-invalid': valid_input == false}" placeholder="Contract or User Address" aria-label="NEO address" aria-describedby="basic-addon2">
					<div class="input-group-append">
						<button type="button" v-on:click="searchForValue" class="btn btn-primary btn-o3-primary">
							Search
						</button>
					</div>
					<div class="invalid-feedback">
						Please provide a valid contract or user address
					</div>
					<div class="valid-feedback">
						{{valid_text}}
					</div>
				</div>
			</div>
		</div>
		<section class="row mx-auto col-8 mx-auto">
			<div class="row" style="width:100%">
				<div class="text-center success-message mb-4" v-if="totalSupply != undefined && totalSupply > 0"> 
						{{ totalSupply }} NFT's Found
				</div>
			</div>
			<div class="row row-grid d-lg-flex" style="width:100%">
				<div class="col-sm-6 col-lg-3 mb-lg-4" v-for="nft in tokens">
					<NFTCard :token_id="nft.token_id" :owner="nft.owner" :uri="nft.uri" :contract="nft.contract"></NFTCard>
				</div>
			</div>

			<div class="d-flex justify-content-center col-10 col-md-6 mx-auto mb-5" v-if="totalSupply != undefined && total_pages > 0">
				<nav aria-label="Token Paging">
					<ul class="pagination">
						<li class="page-item paging-button"><a class="page-link" v-on:click="decrementPage">Previous</a></li>
						<li class="page-item paging-button"><a class="page-link">{{current_page + 1}}</a></li>
						<li class="page-item paging-button"><a class="page-link" v-on:click="incrementPage" >Next</a></li>
					</ul>
				</nav>
			</div>
		</section>

		<section class="row mt-5-1 mb-10 col-10 col-md-6 mx-auto" v-if="totalSupply == undefined || totalSupply == 0"> 
				<div class="landing-info-title-card mb-1 mx-auto">Don't know any NFT addresses or contracts? Check these out!</div>
				<div class="landing-info-card mb-1 mx-auto">Quras Foundry <a href="#" v-on:click="navigateToFoundry">7fe1d36ed60846975e70ec8b6fc0bef08b033107</a></div>
				<div class="landing-info-card mb-1 mx-auto">HashPuppies, the OG NFT <a href="#" v-on:click="navigateToHashPuppy">e7b2046b2412c4c7f1531ce144a73d47c3b272fe</a></div>
				<div class="landing-info-card mb-1 mx-auto">Or mint your own tokens in the Quras Foundry</div>   
		</section>
		<modal ref="modal" :title="modalTitle" :description="modalDescription" :modalAction="modalAction"></modal>
	
</div>
</template>

<script type="text/javascript">
	import NFTCard from './NFTCard.vue'
	import modal from './../modal.vue'

	
	const QurasJs = require('quras-js');
	const testRpcServer = new QurasJs.rpc.RPCClient(QurasJs.CONST.QURAS_NETWORK.MAIN);

	export default {
		components: {
			NFTCard,
			modal
		},
		data: function () {
			return {
				search_value:"",
				valid_input: undefined,
				addressHasNoTokens: false,
				valid_text: "Successfully Validated Contract",
				contract_hash: "",
				address: "",
				token_id: 0,
				image_url:"",
				totalSupply: undefined,
				//Known Contracts should probably be parsed out into a server side method
				known_contracts: [ "7fe1d36ed60846975e70ec8b6fc0bef08b033107",
													"4e2d82efae9bae7e6d3ccb016ab768a607c49c10",
													"0df71b4a31a35b4dc86eeb8d062bc9d8d6235929"
													],
				tokens:[],
				current_page: 0,
				items_per_page: 8,
				total_pages: 0,

				//modal stuff
				modalTitle: "Connect to the test network",
        modalDescription: "Looks like your O3 wallet is set to the mainnet. Currently only test net is available for this app. Please change to testnet in the settings menu",
				modalAction: function() {},

				unknownError: false
			}
		}, 
		methods:{
			buildURIRequest(id, contract) {
				var uriRequest = {
						"scriptHash": contract,
  						"operation": "uri",
  						"args": [
    						{
      							"type": 'Integer',
      							"value": parseInt(id, 10)
      						}
  						],
  						"network": "TestNet"
					}
				return uriRequest
			},

			buildOwnerOfRequest(id, contract) {
				var ownerRequest = {
						"scriptHash": contract,
						"operation": "ownerOf",
						"args": [
							{
								"type": 'Integer',
								"value": parseInt(id, 10)
							}
						],
						"network": "TestNet"
					}
				return ownerRequest
			},

			buildTotalSupplyRequest() {
				var totalSupplyRequest = {
						"scriptHash": this.contract_hash,
						"operation": "totalSupply",
						"args": [],
						"network": "TestNet"
					}
				return totalSupplyRequest
			},
			
			buildGetTokensOfOwnerRequest(scriptHash) {
				var tokensOfOwnerRequest = {
					"scriptHash": scriptHash,
						"operation": "tokensOfOwner",
						"args": [
							{
								"type": 'ByteArray',
								"value": Neon.u.reverseHex(Neon.wallet.getScriptHashFromAddress(this.address))
							}, {
								"type": "Integer",
								"value": 1
							}
						],
					"network": "TestNet"
				}
				return tokensOfOwnerRequest
			},
			convertHexToString(hex) {
    			var str = '';
    			for (var i = 0; (i < hex.length && hex.substr(i, 2) !== '00'); i += 2)
        			str += String.fromCharCode(parseInt(hex.substr(i, 2), 16));
    			return str;
			},

			incrementPage() {
				console.log("Incrementing Page")
				if (this.current_page == this.total_pages - 1) {
					return
				}
				this.current_page = this.current_page + 1
				this.loadTokensForContractPage()
			},

			decrementPage() {
				console.log("Decrementing Page")
				if (this.current_page == 0) {
					return
				}
				this.current_page = this.current_page - 1
				this.loadTokensForContractPage()
			},

			navigateToFoundry() {
				this.search_value = "7fe1d36ed60846975e70ec8b6fc0bef08b033107"
				this.searchForValue()
			},

			navigateToHashPuppy() {
				this.search_value = "e7b2046b2412c4c7f1531ce144a73d47c3b272fe"
				this.searchForValue()
			},
			
			loadAllTokensForAddress() {
				this.$emit('isWaitingForDapi')

				var self = this
				var smartEcoRouter = new smartEco.SmartEcoRouter()
				smartEcoRouter.start()
				self.tokens = []
				self.current_page = 0
				self.total_pages = 0
				self.totalSupply = 0
				var counterForTotalSupply = 0
				for (var i = 0; i < this.known_contracts.length; i++) {
					var req = self.buildGetTokensOfOwnerRequest(self.known_contracts[i])
					Promise.all([smartEcoRouter.invokeRead(req), Promise.resolve(self.known_contracts[i])])
						.then(function(values) {
							var r = values[0]
							var contract = values[1]
							var deserialized = Neon.sc.StackItem.deserialize(r["stack"][0]["value"])
							if (self.known_contracts[self.known_contracts.length-1] == contract 
							&& deserialized["value"].length == 0 && self.totalSupply == 0) {
								self.addressHasNoTokens = true
								self.$emit('isNotWaitingForDapi')

							}
							for (i=0; i< deserialized["value"].length; i++) {
								var idHex = deserialized["value"][i]["value"]["value"][0]["value"]["value"]
								console.log(idHex)
								var id = parseInt(idHex, 16)
								var uriRequest = self.buildURIRequest(id, contract)
								var ownerRequest = self.buildOwnerOfRequest(id, contract)	
								Promise.all([smartEcoRouter.invokeRead(uriRequest), smartEcoRouter.invokeRead(ownerRequest),Promise.resolve(id), Promise.resolve(contract)])
									.then(function(values) {
										console.log(values)
										self.$emit('isNotWaitingForDapi')
										var uri = self.convertHexToString(values[0]["stack"][0]["value"])
										var owner = self.convertHexToString(values[1]["stack"][0]["value"])
										self.tokens.push ({
											"token_id": values[2],
											"uri": uri,
											"owner": QurasJs.wallet.getAddressFromScriptHash(QurasJs.u.reverseHex(values[1]["stack"][0]["value"])),
											"contract": contract
										})
										self.totalSupply +=1
									})
									.catch(function(e) {
										console.log(e)
										self.$emit('isNotWaitingForDapi')
										self.unknownError = true
									})
							}
						})
						.catch(function(e) {
							console.log(e)
							self.$emit('isNotWaitingForDapi')
							self.unknownError = true
						})

				}
			},
			loadTokensForContractPage() {
				var self = this
				console.log("hello")
				self.$emit('isWaitingForDapi')
				var smartEcoRouter = new smartEco.SmartEcoRouter()
				smartEcoRouter.start()
				this.tokens = []
				var startIndex = self.current_page * self.items_per_page + 1
				var endIndex = Math.min(self.current_page * self.items_per_page + self.items_per_page + 1, self.totalSupply + 1)
				var AddingItemInd = startIndex
				for (var i=startIndex; i<endIndex; i++) {
					setTimeout(function(tokenId){
						console.log(tokenId)
						var param = [
							{
								"type": "Integer",
								"value": tokenId
							}
						]
						testRpcServer.invokeFunction(self.contract_hash,"tokenMetadata",param)
						.then((uriValue) => {
							var param_owner = [
								{
									"type": "Integer",
									"value": tokenId
								}
							]
							console.log(tokenId)
							testRpcServer.invokeFunction(self.contract_hash,"ownerOf",param_owner)
							.then((ownerValue) => {
								self.$emit('isNotWaitingForDapi')
								var uri = self.convertHexToString(uriValue["stack"][0]["value"])
								var owner = self.convertHexToString(ownerValue["stack"][0]["value"])
								self.tokens.push ({
									"token_id": tokenId,
									"uri": uri,
									"owner": QurasJs.wallet.getAddressFromScriptHash(QurasJs.u.reverseHex(ownerValue["stack"][0]["value"])),
									"contract": self.contract_hash
								})
							})
							.catch((error) => {
								self.$emit('isNotWaitingForDapi')
								self.unknownError = true
							})
						})
						.catch((error) => {
							self.$emit('isNotWaitingForDapi')
							self.unknownError = true
						})

					}, 1000, i);
				}
			},
			loadAllTokensForContract() {
				var self = this
				self.$emit('isWaitingForDapi')
				this.tokens = []
				self.totalSupply = 0
				testRpcServer.invokeFunction(this.search_value,"deploys",new Array())
				.then((data) => {
					self.totalSupply = parseInt(data.stack[0].value, 16)
					
					self.total_pages = Math.ceil(self.totalSupply / self.items_per_page)
					self.loadTokensForContractPage()
				})
				.catch((error) => {
					console.log(error)
					self.$emit('isNotWaitingForDapi')
					self.unknownError = true
				})
				},
			searchForValue() {
				this.unknownError = false
				this.addressHasNoTokens = false
				/*if (Neon.wallet.isAddress(this.search_value)) {
					this.valid_input = true
					this.valid_text = "Succesfully validated address"
					this.address = this.search_value
					console.log(this.address)
					this.loadAllTokensForAddress()
				} else*/ 
				if (/*QurasJs.utils.isHex(this.search_value) &&*/ this.search_value.length == 42) {
					this.valid_input = true
					this.contract_hash = this.search_value
					this.valid_text = "Succesfully validated contract"
					this.loadAllTokensForContract()
				} else {
					this.valid_input = false
				}
			},
		}, mounted: function () {
				var self = this
				var smartEcoRouter = new smartEco.SmartEcoRouter()
				smartEcoRouter.start()
				smartEcoRouter.getNetworks()
				.then(function(r) {
					console.log(r)
					if (r.networks.includes("TestNet") == false) {
						let element = self.$refs.modal.$el
            $(element).modal('show')
					}
				}).catch(function(e){
					
				})
				this.search_value = this.$route.query.search_value
				this.searchForValue()
    }
	};
</script>

<style>

 .quras-bk-image
 {
	background-image: url('<%= BASE_URL %>../../foundary-background.png');
	background-size: 100%;
 }
 @media (max-width:767px)
 {
	.landing-info {
		color: #4E4E4E;
		font-size: 13px;
		padding-bottom: .5rem;
		text-align: center;
	}

	.landing-info-title {
		color: #4780A0;
		font-size: 13px;
		font-weight: bold;
		padding-bottom: 1.5rem;
		text-align: center;
	}

	.success-message {
		color: #7ED321;
		font-size: 13px;
		font-weight: bold;
		text-align: center;
	}

	.paging-button {
		min-width: 100px;
	}
 }

 @media (min-width:768px)
 {
	.landing-info {
		color: #4E4E4E;
		font-size: 18px;
		padding-bottom: .5rem;
	}

	.landing-info-title {
		color: #4780A0;
		font-size: 18px;
		font-weight: bold;
		padding-bottom: 1.5rem;
	}

	.success-message {
		color: #7ED321;
		font-size: 18px;
		font-weight: bold;
	}

	.paging-button {
		min-width: 100px;
	}
 }

.description-tx
{
	font-size:18px;
}

@media (min-width:768px)
{
	.description-tx
	{
		font-size:1.5rem;
	}
}


</style>
