<template>
<nav class="navbar navbar-expand-lg navbar-light">
	<a class="navbar-brand" href="#"><img src="<%= BASE_URL %>../../quras-logo-light.png" height="30" /></a>
	<div class="collapse navbar-collapse" id="navbar_example_1">

		<ul class="navbar-nav ml-auto">
			<li class="nav-item">
				<router-link to="/explorer" class="nav-link">NFT Explorer</router-link>
			</li>
			<li class="nav-item">
				<router-link to="/foundry" class="nav-link">Token Foundry</router-link>
			</li>
			<li class="nav-item">
				<a class="nav-link" href="https://o3.network" target="_blank">Download</a>
			</li>
		</ul>
	</div>
</nav>
</template>

<script>
export default {
  name: 'AppHeader'
}
</script>

<style>
	.navbar-light .navbar-nav .nav-link {
 		color:	#FFFFFF;
		opacity: 0.7;
	}

	/* change the brand and text color */
.navbar-light .navbar-brand,
.navbar-light .navbar-text {
    color: rgba(255,255,255,.8);
}

/* change the color of active or hovered links */
.navbar-light .nav-item.active .nav-link,
.navbar-light .nav-item:focus .nav-link,
.navbar-light .nav-item:hover .nav-link {
    color:	#FFFFFF;
	opacity: 1.0;
}

.nav-title {
	color: #929292;
	font-family: 'Nunito', sans-serif;
  font-size: .875rem;
  font-weight: 600;
}

	
</style>

