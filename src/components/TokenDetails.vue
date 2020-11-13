<template>
	<div>
		<div class="col-sm-6 col-lg-4 mb-lg-4 mx-auto mt-5">
			<NFTCard  :token_id="$route.params.id" :owner="owner" :uri="uri" :contract="$route.params.contract_hash" :attributes="attributes"></NFTCard>
		</div>
		


		<div class=row>
			<div class="col col-md-6 mx-auto">
				<h2 class="text-center">Why is this cool?</h2>
				<p class="text-center">
					The awesome thing about NFTs is that each token is uniquely numbered and can only be owned
					by one person at a time. 
				</p>
				<p class="text-center">
					Currently this NFT is owned by this address:<br> {{ owner }}.
				</p>
				<p class="text-center">
					If that’s you, you have the ability to transfer it to anyone you like. 
					If not, why not try to mint your own at the <router-link to="/foundry"  >Token Foundry</router-link>.
				</p>
			</div>
		</div>
		<div class="row mt-3 mb-4">
			<div class="col col-md-6 mx-auto">
				<div class="input-group mb-1">
					<input v-model="transfer_address" placeholder="Enter a QURAS address" aria-label="QURAS address" aria-describedby="basic-addon2" class="form-control">
					<div class="input-group-append">
						<button type="button" v-on:click="performTransfer" class="btn btn-primary btn-o3-primary">
							Transfer
						</button>
					</div>
				</div>
			</div>
		</div>
		<div class=row>
			<div class="col col-md-6 mx-auto">
				<h2 class="text-center">What's the point?</h2>
				<p class="text-center">
					Well, currently, this is only a proof of concept, but the ability to buy, sell and trade rare tokens has great potential for many applications, such as gaming, art, music, legal affairs and much more!  
				</p>
			</div>
		</div>
		<modal ref="modal" :title="modalTitle" :description="modalDescription" :modalAction="modalAction"></modal>
		<connectModal ref="connectmodal"></connectModal>
	</div>
</template>

<script type="text/javascript">
	import NFTCard from './NFTExplorer/NFTCard.vue'
	import modal from "./modal.vue"
	import connectModal from "./connectModal.vue"

	const QurasJs = require('quras-js');
	export default {
		name: "TokenDetails",
		components: {
			NFTCard,
			connectModal,
			modal
		}, 
		data: function() {
			return {
				"uri": "",
				"owner": "",
				"attributes":"",
				"transfer_address":"",

				//modal after
				"modalTitle": "",
				"modalDescription": "",
				"modalAction": function() {}
			}
		}, 
		methods: {
			convertHexToString(hex) {
				var str = '';
				for (var i = 0; (i < hex.length && hex.substr(i, 2) !== '00'); i += 2)
					str += String.fromCharCode(parseInt(hex.substr(i, 2), 16));
				return str;
			},
			buildURIRequest() {
				return {
						"scriptHash": this.$route.params.contract_hash,
						"operation": "uri",
						"args": [
							{
								"type": 'Integer',
								"value": parseInt(this.$route.params.id, 10)
							}
						],
						"network": "TestNet"
					}
			},
			buildAttributesRequest() {
				return {
						"scriptHash": this.$route.params.contract_hash,
						"operation": "rwProperties",
						"args": [
							{
								"type": 'Integer',
								"value": parseInt(this.$route.params.id, 10)
							}
						],
						"network": "TestNet"
					}
			},
			buildOwnerOfRequest() {
				return {
						"scriptHash": this.$route.params.contract_hash,
						"operation": "ownerOf",
						"args": [
							{
								"type": 'Integer',
								"value": parseInt(this.$route.params.id, 10)
							}
						],
						"network": "TestNet"
					}
			},
			buildTransferRequest() {
				return {
					"scriptHash": this.$route.params.contract_hash,
						"operation": "transfer",
						"args": [
							{
								"type": 'ByteArray',
								"value": QurasJs.u.reverseHex(QurasJs.wallet.getScriptHashFromAddress(this.transfer_address))
							}, {
								"type": 'Integer',
								"value": parseInt(this.$route.params.id, 10)
							}
							
						],
						"fee": "0", 
						"network": "TestNet"
				}
			}
		}
	}
</script>