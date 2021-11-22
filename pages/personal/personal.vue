<template>
	<view class="personal-container">
		<view class="user-section">
			<image class="bg" src="/static/img/personal/bgImg2.jpg"></image>
			<view class="user-info-box" @click="toLogin">
				<view class="portrait-box">
					<image 
						class="portrait" 
						:src="userInfo.avatarUrl ? userInfo.avatarUrl : '/static/img/personal/missing-face.png'"></image>
				</view>
				<view class="info-box">
					<text class="username">{{ userInfo.nickname ? userInfo.nickname : '游客' }}</text>
				</view>
			</view>
		</view>
		
		<view class="cover-container">
	
			<image class="arc" src="/static/img/personal/arc.png"></image>
			<!-- 个人中心导航 -->
			<view class="nav-section">
				<view class="nav-item"  hover-class="common-hover"  hover-stay-time="50">
					<text class="iconfont icon-xiaoxi"></text>
					<text>我的消息</text>
				</view>
				<view class="nav-item"   hover-class="common-hover" hover-stay-time="50">
					<text class="iconfont icon-myRecommender"></text>
					<text>我的好友</text>
				</view>
				<view class="nav-item"  hover-class="common-hover"  hover-stay-time="50">
					<text class="iconfont icon-gerenzhuye"></text>
					<text>个人主页</text>
				</view>
				<view class="nav-item" hover-class="common-hover"  hover-stay-time="50">
					<text class="iconfont icon-gexingzhuangban"></text>
					<text>个性装扮</text>
				</view>
			</view>
	
			<!-- 个人中心列表 -->
			<view class="personal-content">
				<view class="recent-play-container">
					<text class="title">最近播放</text>
					<!-- 最近播放记录 -->
					<scroll-view v-if="recentPlayList.length" scroll-x class="recent-scroll" enable-flex>
						<view class="recent-item" v-for="item in recentPlayList" :key="item.id">
							<image :src="item.song.al.picUrl"></image>
						</view>
					</scroll-view>
					<view class="title" wx:else>暂无播放记录</view>
				</view>
	
				<view class="card-list">
					<view class="card-item">
						<text class="title">我的音乐</text>
						<text class="more"> > </text>
					</view>
					<view class="card-item">
						<text class="title">我的收藏</text>
						<text class="more"> > </text>
					</view>
					<view class="card-item">
						<text class="title">我的电台</text>
						<text class="more"> > </text>
					</view>
				</view>
			</view>
		</view>
	</view>
		
</template>

<script>
	import request from "../../utils/request.js";
	export default {
		data() {
			return {
				userInfo: {}, // 用户信息
				recentPlayList: [], // 用户播放记录
			}
		},
		async onLoad() {
			this.getUserInfo();
		},
		methods: {
			// 读取用户的基本信息
			getUserInfo() {
				let userInfo = wx.getStorageSync('userInfo');
				if(userInfo) { // 用户登录
					// 更新userInfo的状态
					this.userInfo = JSON.parse(userInfo);
				  
					// 获取用户播放记录
					this.getUserRecentPlayList(this.userInfo.userId)
				}
			},
			toLogin() {
				uni.navigateTo({
					url: '/pages/login/login'
				})
			},
			// 获取用户播放记录的功能函数
			async getUserRecentPlayList(userId) {
				let recentPlayListData = await request('/user/record', {uid: userId, type: 0});
				let index = 0;
				let recentPlayList = recentPlayListData.allData.splice(0, 10).map(item => {
					item.id = index++;
					return item;
				})
				this.recentPlayList = recentPlayList;
			}
		}
	}
</script>

<style scoped lang="scss">
	@import url("./personal.scss")
</style>
