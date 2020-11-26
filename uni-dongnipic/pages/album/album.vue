<template>
	<view>
		<!-- 专题背景开始 -->
		<view class="album_bg">
			<image mode="widthFix" :src="album.cover"></image>
			<view class="album_info">
				<view class="album_name">{{album.name}}</view>
				<view class="album_btn">关注专辑</view>
			</view>
		</view>
		<!-- 专辑作者 -->
		<view class="album_author">
			<view class="album_author_info">
				<image mode="widthFix" :src="album.user.avatar"></image>
				<view class="author_name">{{album.user.name}}</view>
			</view>
			<view class="album_desc">
				<text>{{album.desc}}</text>
			</view>
		</view>
		<view class="album_list">
			<view class="album_item" v-for="(item,index) in wallpaper" :key="item.id">
				<goDetail :list="wallpaper" :index="index">
					<image :src="item.thumb+item.rule.replace('$<Height>',360)" mode="aspectFill"></image>
				</goDetail>
			</view>
		</view>
	</view>
</template>

<script>
	import goDetail from "../components/goDetail.vue"
	export default {
		onLoad(options) {
			this.id = options.id;
			// 调用获取数据的方法
			this.getList()
		},
		components:{
			goDetail
		},
		data() {
			return {
				params: {
					limit: 30,
					order: "new",
					skip: 0,
					first: 1
				},
				id: 0,
				album: {},
				wallpaper: [],
				hasMore: true
			}
		},
		onReachBottom() {
			if (this.hasMore) {
				this.params.skip += this.params.limit;
				this.getList()
			}
		},
		methods: {
			// 获取数据列表
			getList() {
				this.request({
					url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
					data: this.params
				}).then(res => {
					if (Object.keys(this.album).length === 0) {
						this.album = res.res.album;
					}
					if (res.res.wallpaper.length === 0) {
						this.hasMore = false;
						uni.showToast({
							title: "没有更多数据了",
							icon: "none"
						});
						return;
					}

					this.wallpaper = [...this.wallpaper, ...res.res.wallpaper];
				})
			}
		}

	}
</script>

<style lang="scss" scoped>
	.album_bg {
		position: relative;

		image {}

		.album_info {
			padding: 0 20rpx;
			width: 100%;
			position: absolute;
			left: 0;
			bottom: 0;
			display: flex;
			justify-content: space-between;
			height: 80rpx;
			align-items: center;
			color: #fff;

			.album_name {
				font-size: 40rpx;
			}

			.album_btn {
				width: 152rpx;
				height: 60rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				background-color: $color;
				border: 10rpx;
			}
		}
	}

	.album_author {
		padding: 10rpx;

		.album_author_info {
			display: flex;
			align-items: center;
			padding: 10rpx 0;

			image {
				width: 50rpx;
				height: 50rpx;
			}

			.author_name {
				color: #000;
				margin-left: 10rpx;
			}
		}

		.album_desc {}
	}

	.album_list {
		padding: 20rpx 0;
		display: flex;
		flex-wrap: wrap;

		.album_item {
			width: 33.3%;
			border: 3rpx solid #fff;

			image {
				height: 160rpx;
			}
		}
	}
</style>
