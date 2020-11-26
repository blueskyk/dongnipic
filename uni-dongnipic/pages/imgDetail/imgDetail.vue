<template>
	<view>
		<!-- 用户信息开始 -->
		<view v-if="imgDetail.user.avatar" class="user_info">
			<view class="user_icon">
				<image :src="imgDetail.user.avatar" mode="widthFix"></image>
			</view>
			<view class="user_desc">
				<view class="user_name">{{imgDetail.user.name}}</view>
				<view class="user_time">{{imgDetail.zhTime}}</view>
			</view>
		</view>
		<!-- 高清大图 -->
		<view class="high_img">
			<swiper-action @swiperAction="handleSwiperAction">
				<image :src="imgDetail.thumb" mode="widthFix"></image>
			</swiper-action>

		</view>
		<!-- 点赞开始 -->
		<view class="user_rank">
			<view class="rank">
				<text class="iconfont icon-dianzan">{{imgDetail.rank}}</text>
			</view>
			<view class="user_collect">
				<text class="iconfont icon-shoucang">收藏</text>
			</view>
		</view>
		<!-- 下载开始 -->
		<view class="download">
			<view class="download_btn" @click="handleDownLoad">
				下载图片
			</view>
		</view>
	</view>
</template>

<script>
	import moment from "moment";
	import swiperAction from "../components/swiperAction.vue"
	// 设置语言为中文
	moment.locale('zh-cn');
	export default {
		data() {
			return {
				imgDetail: {},
				// 图片的索引
				imgIndex: 0
			}
		},
		components: {
			swiperAction
		},
		onLoad() {
			const {
				imgIndex
			} = getApp().globalData;
			this.imgIndex = imgIndex
			this.getData()
		},
		methods: {
			// 给当前页面赋值
			getData() {
				const {
					imgList
				} = getApp().globalData;
				this.imgDetail = imgList[this.imgIndex];
				// xxx年的格式
				this.imgDetail.zhTime = moment(this.imgDetail.atime * 1000).fromNow();
				// 获取图片详情的id
				this.getComments(this.imgDetail.id)
			},
			getComments(id) {
				this.request({
					url: `http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`
				}).then(res => {
					// 接口取消了,无法实现最新评论
				})
			},
			// 滑动加载图片
			handleSwiperAction(e) {
				const {
					imgList
				} = getApp().globalData;
				// 判断
				if (e.direction === "left" && this.imgIndex < imgList.length - 1) {
					this.imgIndex++
					this.getData()
				} else if (e.direction === "right" && this.imgIndex > 0) {
					this.imgIndex--;
					this.getData()
				} else {
					uni.showToast({
						title: "没有更多数据了",
						icon: "none"
					})
				}
			},
			// 点击下载图片
			async handleDownLoad(e) {
			await uni.showLoading({
					title:"加载中"
				})
				// 将远程文件下载到小程序的内存中
				const res1 = await uni.downloadFile({
					url: this.imgDetail.img
				});
				const {
					tempFilePath
				} = res1[1];
				// 将小程序内存中的临时文件,下载到本地
				const res2 = await uni.saveImageToPhotosAlbum({
					filePath: tempFilePath
				})
			uni.hideLoading();
			await uni.showToast({
				title: "下载成功"
			})
				
			}
		}
	}
</script>

<style lang="scss" scoped>
	.user_info {
		display: flex;
		padding: 20rpx;

		.user_icon {
			padding: 0 20rpx;

			image {
				width: 88rpx;
				border-radius: 50%;
			}
		}

		.user_desc {
			.user_name {
				color: #000;
				font-weight: 600;
			}

			.user_time {
				color: #ccc;
				font-size: 24rpx;
				padding: 10rpx 0;
			}
		}
	}

	.user_rank {
		display: flex;
		height: 80rpx;
		border-bottom: 5rpx solid #eee;

		.rank {
			flex: 1;
			display: flex;
			justify-content: center;
			align-items: center;

			.icon-dianzan {}
		}

		.user_collect {
			flex: 1;
			display: flex;
			justify-content: center;
			align-items: center;

			.icon-shoucang {}
		}
	}

	.download {
		height: 120rpx;
		display: flex;
		align-items: center;
		justify-content: center;

		.download_btn {
			color: #fff;
			width: 95%;
			height: 80%;
			font-size: 50rpx;
			background-color: $color;
			display: flex;
			justify-content: center;
			align-items: center;
		}
	}
</style>
