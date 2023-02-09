<template>
	<view class="content">
		<!-- <image class="logo" src="/static/logo.svg"></image> -->
		<view class="panel">
			<text>{{num}}</text>
		</view>
		<view class="button-area">
			<button v-show="isOver" @click="start">开始抽奖</button>
			<button v-show="!isOver" @click="stop">结束抽奖</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '卡比抽奖',
				num: 40,
				min: 0, //包含
				max: 100, //不包含
				interval: 100, //ms 时间间隔
				isOver: true,
				clock: null,
			}
		},

		onShow() { //只要切换页面就触发
			//先看有没有存储的setting 有就提取
			let setting = uni.getStorageSync('setting') || {
				max: 100,
				min: 0,
				interval: 100
			}
			//初始化
			this.max = setting.max
			this.min = setting.min
			this.interval = setting.interval
		},
		methods: {
			start() {
				this.isOver = false
				this.clock = setInterval(() => {
					this.num = this.getRandomNum()
				}, this.interval)
			},
			getRandomNum() {
				return parseInt(this.min) + Math.floor(Math.random() * (parseInt(this.max) - parseInt(this.min))) //获取随机数,并且向下取整
			},
			stop() {
				clearInterval(this.clock)
				this.isOver = true
			}
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

	.button-area {
		margin-top: 120rpx;
	}

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
</style>
