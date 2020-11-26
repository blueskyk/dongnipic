<template>
	<scroll-view @scrolltolower="handleToLower" scroll-y class="recommend_view" v-if="recommends.length">
		<!-- 推荐开始 -->
		<view class="recommends_wrap">
			<navigator :url="`/pages/album/album?id=${item.id}`" class="recommends_item" v-for="item in recommends" :key="item.id">
				<image :src="item.thumb" mode="widthFix"></image>
			</navigator>
		</view>
		<!-- 月份开始 -->
		<view class="month_wrap">
			<view class="month_title">
				<view class="month_title_info">
					<view class="month_info">
						<text> {{months.DD}} / </text>
						{{months.MM}}月
					</view>
					<view class="month_text">{{months.title}}</view>
				</view>
				<view class="month_title_more">更多 > </view>
			</view>
			<view class="month_content">
				<view class="months_item" v-for="(item,index) in months.items" :key="item.id">
					<goDetail :list="months.items" :index="index"><image :src="item.thumb+item.rule.replace('$<Height>',360)" mode="aspectFill"></image></goDetail>
				</view>
			</view>
		</view>
		<!-- 热门开始 -->
		<view class="hots_wrap">
			<view class="hots_title">
				<text>热门</text>
			</view>
			<view class="hots_contents">
				<view class="hot_item" v-for="(item,index) in hots" :key="item.id">
					<goDetail :list="hots" :index="index"><image :src="item.thumb" mode="widthFix"></image></goDetail>
				</view>
			</view>
		</view>
	</scroll-view>
</template>

<script>
	// 导入日期格式化插件
	import moment from "moment";
	import goDetail from "../goDetail.vue"
	export default {
		data() {
			return {
				recommends: [],
				months: {},
				// 热门数据
				hots: [],
				params: {
					limit: 30,
					order: "hot",
					skip: 0
				},
				// 是否还有下一页
				hasMore: true
			}
		},
		components: {
			goDetail
		},
		mounted() {
			this.getList();
			uni.setNavigationBarTitle({
				title:"推荐"
			})
		},
		methods: {
			handleToLower() {
				if(this.hasMore) {
					this.params.skip += this.params.limit;
					this.getList();
				} else {
					uni.showToast({
						title:"没有更多数据了",
						icon: "none"
					})
				}
				
			},
			// 获取接口数据
			getList() {
				this.request({
					url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
					data: this.params
				}).then(result => {
					// 判断还有没有下一页的数据
					if(result.res.vertical.length === 0) {
						this.hasMore = false;
						uni.showToast({
							title:"没有更多数据了",
							icon: "none"
						});
						return;
					}
					if (this.recommends.length === 0) {
						// 代表第一次发送请求,需要获得具体的时间
						// 推荐数据
						this.recommends = result.res.homepage[1].items
						// 月份数据
						this.months = result.res.homepage[2];
						// 将时间戳转化为具体的日期
						this.months.MM = moment(this.months.stime).format("MM");
						this.months.DD = moment(this.months.stime).format("DD");
					}
					// 热门数据
					// 数组拼接
					this.hots = [...this.hots, ...result.res.vertical];
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.recommend_view {
		height: calc(100vh - 36px);
	}

	.recommends_wrap {
		display: flex;
		flex-wrap: wrap;

		.recommends_item {
			width: 50%;
			border: 5rpx solid #fff;
		}
	}

	.month_wrap {
		.month_title {
			display: flex;
			justify-content: space-between;
			padding: 20rpx;

			.month_title_info {
				color: $color;
				font-size: 30rpx;
				font-weight: 600;
				display: flex;

				.month_info {

					text {
						font-size: 36rpx;
					}
				}

				.month_text {
					color: #666;
					font-size: 34rpx;
					margin-left: 30rpx;
				}
			}

			.month_title_more {
				font-size: 24rpx;
				color: $color;
			}
		}

		.month_content {
			display: flex;
			flex-wrap: wrap;

			.months_item {
				width: 33.3%;
				border: 5rpx solid #fff;
				border-radius: 25%;
			}
		}
	}

	.hots_wrap {


		.hots_title {
			padding: 20rpx;

			text {
				padding-left: 20rpx;
				font-size: 34rpx;
				border-left: 5rpx solid $color;
				font-weight: 600;
			}
		}

		.hots_contents {
			display: flex;
			flex-wrap: wrap;

			.hot_item {
				width: 33.33%;
				border: 5rpx solid #fff;
				border-radius: 25%;

				image {}
			}
		}
	}
</style>
