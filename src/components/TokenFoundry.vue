<template>

<div id="foundry">
    <div class="row mt-7 mb-3">
		<div class="col-10 col-md-6 mx-auto mt-5 mb-5 foundary-description-tx">
			Welcome to Quras Token Foundry! Here you can mint yourself a unique NFT on the Quras NFT contract. The goal of the Token Foundryis to give a simple way to get started with NFTs and start building concepts based around unique collectible tokens.
		</div>
	</div>
    <div class="row">
        <div class="col-6" style="margin-left: auto">
            <div class="row mt-3">
                <div class="col-10">
                    <h6>Select An NFT Contract</h6>
                    <div class="input-group mb-3">
                        <input class="form-control" v-bind:class= "{'is-valid': contract_is_nft, 'is-invalid': contract_is_nft == false}" v-model="contract_hash" placeholder="URI for image" aria-label="NEO address" aria-describedby="basic-addon2">
                        <div class="input-group-append">
                            <button type="button" v-on:click="validateContract" class="btn btn-primary btn-o3-primary">
                                Validate
                            </button>
                        </div>
                        <div class="invalid-feedback">
                            Please provide a valid contract
                        </div>
                        <div class="valid-feedback">
                            This is a valid contract
                        </div>
                    </div>
                </div>
            </div>

            <div class="row mt-3 pb-3">
                <div class="col-10">
                    <h6>Upload a gif from <a target="_blank" href="https://giphy.com/" >Giphy</a></h6>
                    <div class="input-group mb-3">
                        <input v-model="to_search_uri" v-bind:class= "{'is-invalid': uri_is_valid === false}" placeholder="URI for image" aria-label="NEO address" aria-describedby="basic-addon2" class="form-control">
                        <div class="input-group-append">
                            <button type="button" v-on:click="displayURI" class="btn btn-primary btn-o3-primary">
                                Upload
                            </button>
                        </div>
                        <div class="invalid-feedback">
                            Please provide a valid url from Giphy (starts with https://media.giphy.com)
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="row mt-5 pb-3">
                <div class="col-10">
                    <h6>Select The Recipient Address</h6>
                    <div class="input-group mb-3">
                        <input v-model="recipient" v-bind:class= "{'is-invalid': address_is_valid === false}" placeholder="Recipient Address" aria-label="NEO address" aria-describedby="basic-addon2" class="form-control">
                        <div class="invalid-feedback">
                            Please provide a address
                        </div>
                    </div>
                </div>
            </div>
            <div class="row mt-3">
                <div class="col-10">
                    <h6>Enter Your PrivateKey</h6>
                    <textarea v-model="privatekey" v-bind:class= "{'is-invalid': attributes_is_valid === false}" class="form-control" placeholder="This must be private key type" rows="4">
                    </textarea>
                    <div class="invalid-feedback">
                        Please provide a contract owner's privatekey to publish NFT token
                    </div>
                </div>
            </div>
        </div>
    
        <div class="col-2" style="margin-right: auto">
            <NFTCard  :token_id="tokenid" :owner="recipient" :uri="loaded_uri" :contract="contract_hash" :attributes="attributes"></NFTCard>
        </div>
    </div>

    <div class="row justify-content-center mt-6 mb-5">
        <div class="custom-control custom-checkbox">
            <input type="checkbox" class="custom-control-input" id="customCheck1">
            <label class="custom-control-label" for="customCheck1">I agree to the <a href="#" style="font-weight:bold; font-size:1.2rem">terms and conditions</a></label>
        </div>
    </div>

    <div class="row justify-content-center mt-6 mb-5">
        <button type="button" v-on:click="mintToken" class="btn btn-primary btn-o3-primary">
            Mint Token
        </button>
    </div>        
    <modal ref="modal" :title="modalTitle" :description="modalDescription" :modalAction="modalAction"></modal>
    <connectModal ref="connectmodal"></connectModal>
</div>
</template>

<script>
import NFTCard from './NFTExplorer/NFTCard.vue'
import modal from "./modal.vue"
import connectModal from "./connectModal.vue"


const QurasJs = require('quras-js')
const testRpcServer = new QurasJs.rpc.RPCClient(QurasJs.CONST.QURAS_NETWORK.MAIN)




export default {
    components: {
        NFTCard,
        modal,
        connectModal
    },
    data: function () {
        return {
            "to_search_uri": "https://media.giphy.com/media/eN4O2EqeW1lFqRg6dS/giphy.gif",
            "loaded_uri": "https://media.giphy.com/media/eN4O2EqeW1lFqRg6dS/giphy.gif",
            "contract_hash": "0xcc1321a11784192ab50a6141ff6ad267b858c862",
            "tokenid": 100,
            "attributes": "",
            "contract_is_nft": true,
            "recipient": "", 
            "show-modal": false,
            "minted": false,
            "privatekey": "",

            "address_is_valid": undefined,
            "uri_is_valid": undefined,
            "attributes_is_valid": undefined,

            //modal after
            "modalTitle": "",
            "modalDescription": "",
            "modalAction": function() {}
        }
    }, 
    methods:{
        convertHexToString(hex) {
    			var str = '';
    			for (var i = 0; (i < hex.length && hex.substr(i, 2) !== '00'); i += 2)
        			str += String.fromCharCode(parseInt(hex.substr(i, 2), 16));
                return str;
                },
        displayURI() {
            this.loaded_uri = this.to_search_uri
        }, 
        buildSupportedStandardsRequest() {
            var supportedStandardsRequest = { "scriptHash": this.contract_hash,
                "operation": "supportedStandards",
                "args": []  ,
                "network": "TestNet"
            }
            console.log(supportedStandardsRequest)
            return supportedStandardsRequest
        },
        buildMintTokensRequest() {
            var time = new Date()
            var mintTokensRequest = { "scriptHash": this.contract_hash,
                "operation": "mintToken",
                "args": [
                    {"type": "ByteArray", "value": Neon.u.reverseHex(Neon.wallet.getScriptHashFromAddress(this.recipient))},
                    {"type": "ByteArray", "value": "4f3320526f636b73"},
                    {"type": "ByteArray", "value": Neon.u.str2hexstring(this.loaded_uri)},
                    {"type": "ByteArray", "value": Neon.u.str2hexstring(this.attributes)},
                ],
                "network": "TestNet",
                "fee": "0"
            } 
            return mintTokensRequest
        },
        validateContract() {
            console.log("attempting contract validation")
            var self = this
            if (this.contract_hash.length != 42) {
                self.contract_is_nft = false
            } else {
                testRpcServer.invokeFunction(this.contract_hash,"supportedStandards",new Array())
                .then((data) => {
                    var supportedStandards = []
                    for (var i = 0; i < data.stack[0].value.length; i++) {
                        supportedStandards.push(self.convertHexToString(data.stack[0].value[i].value))
                    }
                    if (supportedStandards.includes("NEP-10")) {
                        self.contract_is_nft = true
                    }
                })
                .catch(function(e) {
                    console.log(e) 
                    self.contract_is_nft = false
                })                        
            }
        }, 
        isJsonString(str) {
            try {
                var json = JSON.parse(str);
                return (typeof json === 'object');
            } catch (e) {
                return false;
            }
        },
        validateFields() {
            this.uri_is_valid = true
            this.address_is_valid = true
            this.attributes_is_valid = true
            this.loaded_uri = this.to_search_uri
            //make sure url is a valid image, and if using O3 contract to only allow it via giphy
            if (this.loaded_uri.match(/\.(jpeg|jpg|gif|png)$/) == null || (this.contract_hash == "7fe1d36ed60846975e70ec8b6fc0bef08b033107" && this.loaded_uri.startsWith("https://media.giphy.com") == false)) {
                this.uri_is_valid = false
                return false
            } else if (Neon.wallet.isAddress(this.recipient) == false) {
                this.address_is_valid = false
                return false
            } else if (this.isJsonString(this.attributes) == false) {
                console.log(this.attributes)
                this.attributes_is_valid = false
                return false    
            }
            return true
        },

        mintToken() {
            var params = [
                { "type": "ByteArray",
                "value": QurasJs.u.reverseHex(QurasJs.wallet.getScriptHashFromAddress(this.recipient)) },
                { "type": "String",
                "value": this.to_search_uri }
            ]

            var functionName = {
                "type": "String",
                "value": "mint"
            }

            var pubKey = QurasJs.wallet.getPublicKeyFromPrivateKey(this.privatekey)
            var scriptHash = QurasJs.wallet.getScriptHashFromPublicKey(pubKey)
            var address = QurasJs.wallet.getAddressFromScriptHash(scriptHash)
            
            var contract_hash = this.contract_hash
            if (contract_hash.substr(0, 2) === '0x') {
                contract_hash = contract_hash.substr(2, contract_hash.length - 2)
            }

            QurasJs.api.qurasDB.invokeSmartContract(QurasJs.CONST.QURAS_NETWORK.MAIN,this.privatekey,contract_hash,functionName, params, address)
            .then((data) => {
                console.log(data)
            })
            .catch((error) => {
                console.log(error)
                self.unknownError = true
            })
        }
    }, mounted: function () {
        testRpcServer.invokeFunction("0xcc1321a11784192ab50a6141ff6ad267b858c862","deploys",new Array())
            .then((data) => {
                this.tokenid = parseInt(data.stack[0].value, 16)
                console.log(this.token_id)
            })
            .catch((error) => {
                console.log(error)
            })
    }
}



</script>

<style>
@media (max-width:767px)
 {
    .foundary-description-tx
    {
        text-align: center;
    }
 }

 @media (min-width:768px)
 {
    .foundary-description-tx
    {
        max-width:55%;
        flex: 0 0 55%;
        font-size:1.1rem;
    }
 }
 </style>
