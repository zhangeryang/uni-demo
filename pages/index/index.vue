<template>
	<view class="container">
		<!--banner-->
		<swiper 
			class="swiper-view" 
			indicator-dots 
			indicator-color="#C0C0C0" 
			indicator-active-color="#DCDCDC" 
			autoplay>
			<swiper-item class="swiper-view" v-for="item in bannerList" :key="item.bannerId">
				<image class="img-item" :src="item.pic"></image>
			</swiper-item>
		</swiper>
		
		<!--options-->
		<view class="options-box">
			<view class="item">
				<view class="iconfont icon-tuijian" bindtap="toRecommend"></view>
				<text class="text">每日推荐</text>
			</view>
			<view class="item">
				<view class="iconfont icon-gedan"></view>
				<text class="text">歌单</text>
			</view>
			<view class="item">
				<view class="iconfont icon-paixingbang"></view>
				<text class="text">排行榜</text>
			</view>
			<view class="item">
				<view class="iconfont icon-diantai"></view>
				<text class="text">电台</text>
			</view>
			<view class="item">
				<view class="iconfont icon-zhibo"></view>
				<text class="text">直播</text>
			</view>
		</view>
		
		<!-- #ifdef H5 -->
		<!--每日推荐-->
		<view class="recommend panel">
			<NavHeader title="推荐歌曲" content="为你精心推荐"></NavHeader>
			<scroll-view class="scroll-view" scroll-x>
				<view class="scroll-view-item" v-for="item in recommendList" :key="item.id">
					<image class="item-img" :src="item.picUrl"></image>
					<text class="item-text">{{ item.name }}</text>
				</view>
			</scroll-view>
		</view>
		<!-- #endif -->
		
		<!--排行榜-->
		<view class="top-list panel">
			<NavHeader title="排行榜" content="热歌风向标"></NavHeader>
			<swiper class="top-list-swiper" circular next-margin="50rpx">
				<swiper-item v-for="item in topList" :key="item.name">
					<view class="swiper-item">
					<view class="title">{{ item.name }}</view>
					<view class="music-item" v-for="(musicItem, index) in item.tracks" :key="musicItem.id">
						<image :src="musicItem.al.picUrl"></image>
						<text class="count">{{ index + 1 }}</text>
						<text class="music-name">{{ musicItem.name }}</text>
					</view>
					</view>
				</swiper-item>
			</swiper>
		</view>
		
	</view>
</template>

<script>
	import request from "../../utils/request.js";
	import NavHeader from "../../components/NavHeader/NavHeader.vue";
	export default {
		components:{
			NavHeader
		},
		data() {
			return {
				bannerList: [], // 轮播图数据
				recommendList: [], // 推荐歌单
				topList: [], // 排行榜数据
			}
		},
		onLoad() {
			this.getBannerList();
			// #ifdef H5
			this.getRecommendList();
			// #endif
			this.getTopList();
		},
		methods: {
			// 获取banner
			async getBannerList() {
				const bannerListData = await request('/banner', { type: 2 });
				this.bannerList = bannerListData.banners; 
			},
			// 获取推荐歌单数据
			async getRecommendList() {
				const recommendListData = await request('/personalized', { limit: 10 });
				this.recommendList = recommendListData.result; 
			},
			async getTopList() {
				let index = 0;
				let resultArr = [];
				while (index < 5) {
					let topListData = await request('/top/list', { idx: index++ });
					let topListItem = {
						name: topListData.playlist.name, 
						tracks: topListData.playlist.tracks.slice(0, 3)
					}
					resultArr.push(topListItem);
					// 不需要等待五次请求全部结束才更新，用户体验较好，但是渲染次数会多一些
					this.topList = resultArr;
				}
			}
		}
	}
</script>

<style scoped lang="scss">
	@import url("./index.scss");
</style>
