<script>
	import request from "./utils/request.js";
	export default {
		globalData:{
			userInfo: {}
		},
		onLaunch: function() {
			
		},
		onShow: function() {
			this.login();
		},
		onHide: function() {
			
		},
		methods: {
			async login() {
				let result = await request('/getOpenId');
				console.log(result, "========");
				uni.setStorageSync('user_token', result);
			}
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import '/static/iconfont/iconfont.wxss';
</style>
