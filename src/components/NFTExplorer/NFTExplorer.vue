<template>
<div id="explorecard">
	<div class="row mt-10">
		<div class="col-4" style="margin-left:auto; padding-left:3rem; margin-right:3rem">
			<div class="row ">
				<div class="" style="font-size:32px	;color:#4780A0;font-weight:bold;text-transform: uppercase;">
					Welcome to Quras’s Non fungible token (NFT) explorer and minter. 
				</div>
			</div>

			<div class="row mt-5">
				<div class="" style="font-size:18px;">
					This POC is to help developers get started with NFT’s in the NEO Smart Economy.
					<a href="#">Learn More</a>
				</div>
			</div>

			<div class="row mt-5">
				<div style="width:100%">
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

			<section class="row mt-5 mb-10" v-if="totalSupply == undefined || totalSupply == 0"> 
				<div class="">
					<div class="landing-info-title mb-1">Don't know any NFT addresses or contracts? Check these out!</div>
					<div class="landing-info mb-1">Quras Foundry <a href="#" v-on:click="navigateToFoundry">7fe1d36ed60846975e70ec8b6fc0bef08b033107</a></div>
					<div class="landing-info mb-1">Or mint your own tokens in the Quras Foundry</div>
				</div>   
			</section>
			<modal ref="modal" :title="modalTitle" :description="modalDescription" :modalAction="modalAction"></modal> 	
		</div>
		<div class="col-3" style="margin-right:auto;margin-left:3rem" >
			<img src="<%= BASE_URL %>../../ntf-token-marks.png" class="mt-4" width="100%"/>
		</div>
	</div>
</div>
</template>

<script type="text/javascript">
	import modal from './../modal.vue'

	export default {
		components: {
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
			searchForValue() {
				window.location.href = "/#/explorecard?search_value=" + this.search_value;
			},
			navigateToFoundry() {
				this.search_value = "7fe1d36ed60846975e70ec8b6fc0bef08b033107"
				this.searchForValue()
			},

			navigateToHashPuppy() {
				this.search_value = "e7b2046b2412c4c7f1531ce144a73d47c3b272fe"
				this.searchForValue()
			},
		}, mounted: function () {
    }
	};
</script>

<style>

 .quras-bk-image
 {
	background-image: url('<%= BASE_URL %>../../foundary-background.png');
	background-size: 100%;
 }
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

</style>
