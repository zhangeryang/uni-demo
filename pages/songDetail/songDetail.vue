<template>
	<view class="song-detail-Container">
	    <view class="author">{{ song.ar[0].name }}</view>
	    <!-- 用一个圆模拟底座-->
	    <view class="circle"></view>
	    <image :class="['needle', isPlay && 'needle-rotate']" src="/static/img/songDetail/needle.png"></image>
	
	    <view :class="['disc-container', isPlay && 'disc-animation']">
	        <image class="disc" src="/static/img/songDetail/disc.png"></image>
	        <image class="music-img" :src="song.al.picUrl"></image>
	    </view>
	
	
	    <!-- 底部控制播放区域 -->
	    <view class="music-control">
	        <text class="iconfont icon-bofang-xunhuanbofang"></text>
	        <text class="iconfont icon-shangyishoushangyige" id="pre" @click="handleSwitch"></text>
	        <text :class="['iconfont', isPlay ? 'icon-zanting' : 'icon-bofang', big]" @click="handleMusicPlay"></text>
	        <text class="iconfont icon-xiayigexiayishou" id="next" @click="handleSwitch"></text>
	        <text class="iconfont icon-bofangliebiao"></text>
	    </view>
	
	    
	</view>
</template>

<script>
	import request from "../../utils/request.js";
	export default {
		data() {
			return {
				isPlay: false, // 音乐是否播放
				song: {}, // 歌曲详情对象
				musicId: '', // 音乐id
				musicLink: '', // 音乐的链接
			}
		},
		async onLoad() {
			
		},
		methods: {
			// 点击播放、暂停
			handleMusicPlay() {
				
			},
			// 点击切歌
			handleSwitch(event) {
				// // 获取切歌的类型: pre 上一首  next 下一首
				// let type = event.currentTarget.id;
				
				// // 关闭当前播放的音乐
				// this.backgroundAudioManager.stop();
				// // // 订阅来自recommendSong页面发布的musicId消息
				// PubSub.subscribe('musicId', (msg, musicId) => {
				// 	// 获取音乐详情信息
				// 	this.getMusicInfo(musicId);
				// 	// 自动播放当前的音乐
				// 	this.musicControl(true, musicId);
				// 	// 取消订阅
				// 	PubSub.unsubscribe('musicId');
				// })
				// // 发布消息数据给recommendSong页面
				// PubSub.publish('switchType', type)
			},
		}
	}
</script>

<style>
</style>
