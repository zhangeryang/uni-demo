<template>
	<view class="song-detail-container">
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
	        <text class="iconfont icon-shangyishoushangyige" @click="handleSwitch"></text>
	        <text :class="['iconfont', isPlay ? 'icon-zanting' : 'icon-bofang', big]" @click="handleMusicPlay"></text>
	        <text class="iconfont icon-xiayigexiayishou" @click="handleSwitch"></text>
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
		async onLoad(options) {
			this.musicId = options.musicId;
			// 获取音乐详情
			this.getMusicInfo(this.musicId);
			this.handleMusicPlay();
			
			// 创建控制音乐播放的实例
			// 直接通过this给页面实例绑定属性，页面内都可以访问
			this.backgroundAudioManager = uni.getBackgroundAudioManager();
	
			// 监视音乐播放/暂停/停止
			this.backgroundAudioManager.onPlay(() => {
				this.changePlayState(true);
				// 修改全局音乐播放的状态
				appInstance.globalData.musicId = musicId;
			});
			this.backgroundAudioManager.onPause(() => {
				this.changePlayState(false);
			});
			this.backgroundAudioManager.onStop(() => {
				this.changePlayState(false);
			});
		},
		methods: {
			// 获取音乐详情的功能函数
			async getMusicInfo(musicId) {
				let songData = await request('/song/detail', { ids: musicId });
				this.song = songData.songs[0];
				uni.setNavigationBarTitle({
					title: this.song.name
				})
			},
			// 点击播放、暂停
			handleMusicPlay() {
				let isPlay = !this.isPlay;
				
				let { musicId, musicLink } = this;
				this.musicControl(isPlay, musicId, musicLink);
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
			
			// 控制音乐播放/暂停的功能函数
			async musicControl(isPlay, musicId, musicLink) {
				if(isPlay){ // 音乐播放
					// 避免重复请求音乐链接
					if(!musicLink){
						// 获取音乐播放链接
						let musicLinkData = await request('/song/url', {id: musicId});
						this.musicLink = musicLinkData.data[0].url;
					}
					// 设置当前音乐
					this.backgroundAudioManager.src = musicLink;
					this.backgroundAudioManager.title = this.song.name;
				}else { // 暂停音乐
					this.backgroundAudioManager.pause();
				}
				
			},

		}
	}
</script>

<style scoped lang="scss">
	@import url("./songDetail.scss")
</style>
