<template>
	<view @click="handleClick">
		<slot></slot>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		props: {
			list: Array,
			index: Number
		},
		methods: {
			handleClick() {
				// 将数据缓存下来
				getApp().globalData.imgList = this.list;
				getApp().globalData.imgIndex = this.index;
				// 跳转路径
				uni.navigateTo({
					url:"/pages/imgDetail/imgDetail"
				})
			}
		}
	}
</script>

<style>

</style>
