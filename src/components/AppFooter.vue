<template>
<div>
    <section class="slice slice-lg">
        <div class="slice-container">
            <div class="card shadow hover-shadow-lg border-0 position-relative">
                <div class="card-body">
                <div class="d-flex align-items-start mb-3">
                    <div class="icon-text">
                        <h2 class="text-blue bold">Building something awesome? Integrate with Quras!</h2>
                        <p class="text-dark-blue mb-0 padding-top-30">With Quras’s dAPI, it’s so simple to hook your app up to the blockchain. Quras will handle all the wallet and chain 
                            interaction heavy lifting, so you can focus on making your app awesome                        </p>
                    </div>
                </div>
                <div class="d-flex padding-top-30">
                    <a href="https://neodapitestbed.o3.app/" target="_blank" class="btn btn-primary btn-o3-primary">Check out the dAPI Today!</a>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <footer id="footer-main" class="footer-quras zindex-101">
        <div class="footer footer-quras-dark">
            <div class="container">
                    <div class="row">
                        <a href="/" class="footer-quras-logo">
                            <img src="<%= BASE_URL %>../../quras-logo-blue.png" style="height: 40px;">
                        </a>
                    </div>
                    <div class="row">
                    <div class="col-lg-3 mb-5 mb-lg-0">
                        <p class="footer-quras-twitt-logo ">We are here!</p>
                        <div class="row">
                            <div class="col">
                                <a href="\" class="footer-quras-logo-btn">
                                    <img src="<%= BASE_URL %>../../link_0.png" class="footer-quras-logo-btn-img" style="height: 15px;">
                                </a>
                            </div>
                            <div class="col">
                            <a href="\" class="footer-quras-logo-btn">
                                <img src="<%= BASE_URL %>../../link_1.png" class="footer-quras-logo-btn-img" style="height: 15px;">
                            </a>
                            </div>
                            <div class="col">
                            <a href="\" class="footer-quras-logo-btn">
                                <img src="<%= BASE_URL %>../../link_2.png" class="footer-quras-logo-btn-img" style="height: 15px;">
                            </a>
                            </div>
                            <div class="col">
                            <a href="\" class="footer-quras-logo-btn">
                                <img src="<%= BASE_URL %>../../link_3.png" class="footer-quras-logo-btn-img" style="height: 15px;">
                            </a>
                            </div>
                            <div class="col">
                            <a href="\" class="footer-quras-logo-btn">
                                <img src="<%= BASE_URL %>../../link_4.png" class="footer-quras-logo-btn-img" style="height: 15px;">
                            </a>
                            </div>
                            <div class="col">
                            <a href="\" class="footer-quras-logo-btn">
                                <img src="<%= BASE_URL %>../../link_5.png" class="footer-quras-logo-btn-img" style="height: 15px;">
                            </a>
                            </div>
                            <div class="col">
                            <a href="\" class="footer-quras-logo-btn">
                                <img src="<%= BASE_URL %>../../link_6.png" class="footer-quras-logo-btn-img" style="height: 15px;">
                            </a>
                            </div>
                        </div>
                    </div>
                    <div class="col-lg-3 col-6 col-sm-4 ml-lg-auto mb-5 mb-lg-0">
                        <h6 class="heading mb-3">Community</h6>
                        <ul class="list-unstyled">
                            <li><a href="#">Quras Telegram Group</a>
                            </li>
                            <li><a href="#">Face</a>
                            </li>
                            <li><a href="#">Twitter</a>
                            </li>
                        </ul>
                        
                    </div>
                    
                    <div class="col-lg-3 col-sm-4 mb-5 mb-lg-0">
                        <h6 class="heading mb-3">Quras</h6>
                        <ul class="list-unstyled">
                            <li><a href="#">IOS</a>
                            </li>
                            <li><a href="#">Android</a>
                            </li>
                            <li><a href="#">macOS</a>
                            </li>
                            <li><a href="#">Windows</a>
                            </li>
                        </ul>
                    </div>
                
                    <div class="col-lg-2 col-sm-4 mb-5 mb-lg-0">
                        <h6 class="heading mb-3">Company</h6>
                        <ul class="list-unstyled">
                            <li><a href="#">Home</a>
                            </li>
                            <li><a href="#">Team</a>
                            </li>
                            <li><a href="#">Support</a>
                            </li>
                        </ul>
                    </div>
                </div>
                
            </div>
            <div class="row align-items-center justify-content-md-between py-4 mt-4 footer-quras-color-dark-btm">
                <div class="col-md-1">
                    <div class="copyright text-sm font-weight-bold text-center text-md-left">
                        <a href="#" class="font-weight-bold" target="_blank">Terms</a>
                    </div>
                </div>

                <div class="col-md-1">
                    <div class="copyright text-sm font-weight-bold text-center text-md-left">
                        <a href="#" class="font-weight-bold" target="_blank">Privacy</a>
                    </div>
                </div>

                <div class="col-md-2">
                    <div class="copyright text-sm font-weight-bold text-center text-md-left">
                        <a href="#" class="font-weight-bold" target="_blank">Cookie Policy</a>
                    </div>
                </div>

                <div class="col-md-2 ml-lg-auto">
                    <div class="copyright text-sm font-weight-bold text-center text-md-left">
                        <a href="#" class="font-weight-bold" target="_blank">hi@quras.io</a>
                    </div>
                </div>

            </div>
            </div>
        </footer>
    </div>
</template>

<script>
export default {
  name: 'AppFooter'
}
</script>