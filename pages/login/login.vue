<template>
	<view class="login-container">
		<view class="wrapper">
			<view class="left-top-sign">LOGIN</view>
			<view class="welcome">欢迎回来！</view>
	
			<view class="input-content">
				<view class="input-item">
					<text class="tit">手机号码</text>
					<input  
						type="text" 
						placeholder="请输入手机号码" 
						:value="phone"
						@blur="handleInputBlur('phone', $event)"/>
				</view>
				<view class="input-item">
					<text class="tit">密码</text>
					<input 
						type="password"  
						placeholder="请输入密码"
						:value="password"
						@blur="handleInputBlur('password', $event)"/>
				</view>
			</view>
			<button class="btn confirm-btn" @click="handleLogin">登录</button>
			<!-- #ifdef MP-WEIXIN -->
			<button class="btn weixin-btn" @click="handleWeixinLogin">微信快捷登录</button>
			<!-- #endif -->
			<view class="forget-section">忘记密码?</view>
		</view>
	</view>
</template>

<script>
	import request from "../../utils/request.js";
	import app from "../../App.vue";
	export default {
		data() {
			return {
				phone: '',
				password: ''
			}
		},
		async onLoad() {
			
		},
		methods: {
			handleInputBlur(type, event) {
				this[type] = event.detail.value;
			},
			
			// 登录
			async handleLogin() {
				// 1. 收集表单项数据
				let { phone, password } = this;
				
				// 2. 前端验证
				if(!phone) {
					// 提示用户
					uni.showToast({
						title: '手机号不能为空',
						icon: 'none'
					})
					return;
				}
				// 定义正则表达式
				let phoneReg = /^1(3|4|5|6|7|8|9)\d{9}$/;
				if(!phoneReg.test(phone)) {
					uni.showToast({
						title: '手机号格式错误',
						icon: 'none'
					})
					return;
				}
				
				if(!password) {
					uni.showToast({
						title: '密码不能为空',
						icon: 'none'
					})
					return;
				}
				
				// 接口验证
				let result = await request('/login/cellphone', { phone, password, isLogin: true })
				if(result.code === 200) { // 登录成功
					uni.showToast({
						title: '登录成功'
					})
					// 将用户的信息存储至本地
					uni.setStorageSync('userInfo', JSON.stringify(result.profile))
					
					// 跳转至个人中心personal页面
					uni.reLaunch({
						url: '/pages/personal/personal'
					})
				}else if(result.code === 400) {
					uni.showToast({
						title: '手机号错误',
						icon: 'none'
					})
				}else if(result.code === 502) {
					uni.showToast({
						title: '密码错误',
						icon: 'none'
					})
				}else {
					uni.showToast({
						title: '登录失败，请重新登录',
						icon: 'none'
					})
				}
			},
			
			async handleWeixinLogin() {
				uni.getSetting({
					success(res) {
						if (res.authSetting['scope.userInfo']) {
							// 用户信息已授权，获取用户信息
							uni.getUserInfo({
								success(res) {
									// this.phone = "17629109563";
									// this.password = "wyy123456";
									// this.handleLogin();
									uni.reLaunch({
										url: '/pages/personal/personal'
									})
								},
								fail() {
									console.log("获取用户信息失败")
								}
							})
						} else if (!res.authSetting['scope.userInfo']) {
							console.log("需要点击按钮手动授权")
						}
					},
					fail() {
						console.log("获取已授权选项失败")
					}
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	@import url("./login.scss")
</style>
