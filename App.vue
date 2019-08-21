<script>
	import token from "./common/token.js"
	export default {
		onLaunch: function() {	
			var hasToken = token.verify();
			if(!hasToken){
			 	uni.reLaunch({
			 		url: '/pages/index/index'
			 	});
			};
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	body, html {
		-webkit-user-select: none;
		user-select: none;
		width: 100%;
		height:auto;
	}
	/* @import "/assets/style/quill.css"; */
	/*每个页面公共css */
/* 	@import "/assets/style/quill.css"; */
	
</style>
