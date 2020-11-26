<template>
	<view class="home_category">
		<navigator :url="`/pages/imageCategory/imageCategory?id=${item.id}`" class="cate_item" v-for="item in category" :key="item.id">
			<image :src="item.cover" mode="aspectFill"></image>
			<view class="cate_name">{{item.name}}</view>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				category: []
			}
		},
		mounted() {
			uni.setNavigationBarTitle({
				title: "分类"
			});
			this.getList()
		},
		methods: {
			getList() {
				this.request({
					url: "http://157.122.54.189:9088/image/v1/vertical/category"
				}).then(res => {
					console.log(res)
					this.category = res.res.category
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.home_category {
		display: flex;
		flex-wrap: wrap;
		.cate_item {
			width: 33.3%;
			position: relative;
			border: 5rpx solid #fff;
			image {
				height: 240rpx;
			}
			.cate_name {
				position: absolute;
				width: 100%;
				display: flex;
				align-items: center;
				height: 40rpx;
				left: 0;
				bottom: 0;
				color: #fff;
				background-image: linear-gradient(to right top ,rgba(0,0,0,.2),rgba(0,0,0,0));
				font-size: 40rpx;
				padding-left: 20rpx;
			}
		}
	}
</style>
