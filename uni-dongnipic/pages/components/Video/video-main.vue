<template>
	<scroll-view enable-flex scroll-y @scrolltolower="handlescrolltolower" class="video_main">
		<view class="video_item" v-for="item in videowp" :key="item.id" @click="handleGoVideo(item)">
			<image mode="widthFix" :src="item.img"></image>
		</view>
	</scroll-view>
</template>

<script>
	export default {
		data() {
			return {
				videowp: [],
				// 是否有下一页
				hasMore: true
			}
		},
		props: {
			urlObj: {
				type: Object
			}
		},
		watch: {
			urlObj() {
				this.videowp = []
				this.getList()
			}
		},
		mounted() {
			this.getList()
		},
		methods: {
			getList() {
				this.request({
					url: this.urlObj.url,
					data: this.urlObj.params
				}).then(res => {
					if (res.res.videowp.length === 0) {
						// 没有下一页的数据
						this.hasMore = false;
						uni.showToast({
							title: "没有更多数据了",
							icon: "none"
						})
						return;
					}
					this.videowp = [...this.videowp, ...res.res.videowp];
					console.log(this.videowp)
				})
			},
			handlescrolltolower() {
				if (this.hasMore) {
					this.urlObj.skip += this.urlObj.limit;
					this.getList();
				} else {
					uni.showToast({
						title: "没有更多数据了",
						icon: "none"
					})
				}
			},
			handleGoVideo(item) {
				// 将传递的数据存入到全局共享数据
				getApp().globalData.video = item;
				uni.navigateTo({
					url:"/pages/videoPlay/videoPlay"
				})
			}
		}
	}
</script>

<style lang="scss">
	.video_main {
		height: calc(100vh - 36px);
		display: flex;
		flex-wrap: wrap;

		.video_item {
			width: 33.33%;
			border: 5rpx solid #fff;

			image {}
		}
	}
</style>
