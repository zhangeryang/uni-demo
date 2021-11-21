<template>
	<view class="header">
	    <text class="title">{{title}}</text>
	    <view>
	        <text class="description">{{content}}</text>
	        <text class="getmore">查看更多</text>
	    </view>
	</view>
</template>

<script>
	export default {
		props:{
			title: {
				type: String,
				value: '我是title默认值'
			},
			content: {
				type: String,
				value: '我是content默认值'
			}
		},
		data() {
			return {
				
			}
		},
		
		methods: {
			
		},
		
	}
</script>

<style scoped lang="scss">
	@import url("./NavHeader.scss");
</style>
