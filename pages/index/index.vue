<template>
	<view class="content">
		<!-- <image class="logo" src="/static/logo.svg"></image> -->

		<unicloud-db ref="udb" v-slot:default="{data, pagination, loading, hasMore, error}" :collection="collectionList"
			field="title,photo">
			<view v-if="error">{{error.message}}</view>
			<view v-else-if="data">
				<uni-list>
					<uni-list-item v-for="(item, index) in data" :key="index">
						<template v-slot:body>
							<view class="item">
								<!-- 此处默认显示为_id，请根据需要自行修改为其他字段 -->
								<!-- 如果使用了联表查询，请参考生成的 admin 项目中 list.vue 页面 -->
								<view>
									<image :src="item.photo.url" mode="aspectFill"></image>
								</view>
								<view>
									{{item.title}}
								</view>
							</view>
						</template>
					</uni-list-item>
				</uni-list>
			</view>
			<uni-load-more :status="loading?'loading':(hasMore ? 'more' : 'noMore')"></uni-load-more>
		</unicloud-db>
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
				collectionList: "lottery",
				title: '卡比抽奖',
				num: 40,
				min: 0, //包含
				max: 100, //不包含
				interval: 100, //ms 时间间隔
				isOver: true,
				clock: null,
				loadMore: {
					contentdown: '',
					contentrefresh: '',
					contentnomore: ''
				}
			}
		},
		onPullDownRefresh() {
			this.$refs.udb.loadData({
				clear: true
			}, () => {
				uni.stopPullDownRefresh()
			})
		},
		onReachBottom() {
			this.$refs.udb.loadMore()
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
				return parseInt(this.min) + Math.floor(Math.random() * (parseInt(this.max) - parseInt(this
					.min))) //获取随机数,并且向下取整
			},
			stop() {
				clearInterval(this.clock)
				this.isOver = true
			}
		}
	}
</script>

<style lang="scss">
	.uni-list {
		display: flex;
		flex-wrap: wrap;
		flex-direction: row;
	}

	.item {
		text-align: center;

		image {
			width: 200rpx;
			height: 200rpx;

		}
	}

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
