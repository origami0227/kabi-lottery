<template>
	<view class="content">
		<!-- <image class="logo" src="/static/logo.svg"></image> -->
		<view class="panel">
			<text>{{num}}</text>
		</view>
		<view class="button-area">
			<button v-show="isOver">开始抽奖</button>
			<button v-show="!isOver">结束抽奖</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '卡比抽奖',
				num: 40,
				isOver: true,
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style lang="scss">
	.panel {
		width: 600rpx;
		height: 400rpx;
		border: 1rpx solid $uni-border-color;
		border-radius: 8rpx;
		margin-top: 60rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		box-shadow: 0 0 40rpx 0rpx rgba(0, 0, 0, 0.1);

		text {
			font-size: 180rpx;
			
		}
	}
	.button-area{
		margin-top: 120rpx;
	}

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
</style>
