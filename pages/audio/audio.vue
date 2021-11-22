<template>
	<view class="audio-container">
	    <!-- 头部  -->
	    <view class="header">
	        <image src="/static/img/recommend/background.png"></image>
	    </view>
	
	    <!-- 列表区域 -->
	    <view class="list-container">
	        <view class="list-header">
	            <text>播放全部</text>
	            <text class="change-more">多选</text>
	        </view>
	
	        <!-- 内容区 -->
	        <scroll-view scroll-y class='listScroll'>
	            <view 
	                class="scroll-item" 
	                v-for="(item, index) in recommendList" 
	                :key="item.id"
					:song="item"
					:index="index"
	                @click="toSongDetail">
	                <image :src="item.album.picUrl"></image>
	                <view class="music-info">
	                    <text class="music-name">{{ item.name }}</text>
	                    <text class="author">{{ item.artists[0].name }}</text>
	                </view>
	                <text class="iconfont icon-gengduo"></text>
	            </view>
	        </scroll-view>
	    </view>
	</view>
</template>

<script>
	import request from "../../utils/request.js";
	export default {
		data() {
			return {
				recommendList: [], // 推荐列表数据
				index: 0, // 点击音乐的下标
			}
		},
		async onLoad() {
			// 判断用户是否登录
			let userInfo = uni.getStorageSync('userInfo');
			// if(!userInfo) {
			// 	uni.showToast({
			// 		title: '请先登录',
			// 		icon: 'none',
			// 		success: () => {
			// 			// 跳转至登录界面
			// 			uni.reLaunch({
			// 				url: '/pages/login/login'
			// 			})
			// 		}
			// 	})
			// }
			this.getRecommendList();
		},
		methods: {
			// 获取用户每日推荐数据
			async getRecommendList() {
				let recommendListData = await request('/recommend/songs');
				this.recommendList = recommendListData.recommend;
			},
			
			// 跳转至songDetail页面
			toSongDetail(event) {
				consoel.log(event, "===========")
				// let { song, index } = event.currentTarget.dataset;
				// this.index = index;
				// // 路由跳转传参： query参数
				// uni.navigateTo({
				// 	url: '/pages/songDetail/songDetail?musicId=' + song.id
				// })
			},
		}
	}
</script>

<style scoped lang="scss">
	@import url("./audio.scss")
</style>
