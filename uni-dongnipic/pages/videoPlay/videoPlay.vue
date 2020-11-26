<template>
	<view class="video_play">
		<image :src="videoObj.img"></image>
		<!-- 工具栏 -->
		<view class="video_tool">
			<view @click="handleMuted" :class="['iconfont',muted?'icon-jingyin':'icon-shengyin']"></view>
			<view class="iconfont icon-zhuanfa">
				<button open-type="share"></button>
			</view>
		</view>
		<!-- 视频开始 -->
		<view class="video_wrap">
			<video :muted="muted" :src="videoObj.video" objectFit="fill" controls></video>
		</view>
		<!-- 下载高清 -->
		<view class="download">
			<view class="download_btn" @click="handleDownLoad">下载高清</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				videoObj: {},
				// 是否静音
				muted: false
			}
		},
		methods: {
			// 控制声音的静音与播放
			handleMuted() {
				this.muted = !this.muted
			},
			// 下载高清
			async handleDownLoad() {
				uni.showLoading({
					title:"下载中"
				});
				const {tempFilePath} = (await uni.downloadFile({url:this.videoObj.video}))
			await uni.saveVideoToPhotosAlbum({
					filePath:tempFilePath
				});
				uni.hideLoading();
			await uni.showToast({
					title:"下载成功"
				})
			}
		},
		onLoad() {
			console.log(getApp().globalData)
			this.videoObj = getApp().globalData.video;
		}
	}
</script>

<style lang="scss">
	.video_play {
		position: relative;

		image {
			position: absolute;
			height: 100vh;
			width: 100vw;
			z-index: -1;
			filter: blur(10px);
		}

		.video_tool {
			height: 80rpx;
			display: flex;
			justify-content: flex-end;

			.iconfont {
				width: 80rpx;
				color: #fff;
				font-size: 50rpx;
				border-radius: 50%;
				background-color: rgba(0, 0, 0, .2);
				display: flex;
				justify-content: center;
				align-items: center;
				margin-right: 20rpx;
			}

			.icon-zhuanfa {
				position: relative;

				button {
					position: absolute;
					width: 100%;
					height: 100%;
					opacity: 0;
				}
			}
		}

		.video_wrap {
			display: flex;
			justify-content: center;

			video {
				width: 360rpx;
				height: 600rpx;
			}
		}

		.download {
			margin-top: 30rpx;
			display: flex;
			justify-content: center;

			.download_btn {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 360rpx;
				height: 80rpx;
				color: #fff;
				background-color: rgba(0, 0, 0, .6);
				border-radius: 40rpx;
				border: 3rpx solid #fff;
			}
		}
	}
</style>
