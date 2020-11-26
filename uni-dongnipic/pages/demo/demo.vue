<template>
	<view @touchstart="handleTouchStart" @touchend="handleTouchEnd">
		测试页面
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 开始的时间
				startTime: 0,
				// 按下的坐标
				startX: 0,
				startY: 0
			}
		},
		methods: {
			handleTouchStart(e) {
				// 开始按下的时间
				this.startTime = Date.now();
				this.startX = e.changedTouches[0].clientX;
				this.startY = e.changedTouches[0].clientY;
			},
			handleTouchEnd(e) {
				const endTime = Date.now();
				const endX = e.changedTouches[0].clientX;
				const endY = e.changedTouches[0].clientY;
				// 判断用户操作时长是否合法
				if(endTime - this.startTime >2000) {
					uni.showToast({
						title:"操作时间过长",
						icon:"none",
						mask:true
					})
					return;
				}
				// 滑动的方向
				let direction = ""
				// 判断用户滑动的距离
				if(Math.abs(endX-this.startX) > 10) {
					direction = endX - this.startX > 0?"right": "left"
				} else {
					uni.showToast({
						title: "超出屏幕范围",
						icon:"none",
						mask:true
					})
				}
				console.log(direction)
			}
		}
	}
</script>

<style>
  view {
	  width: 100%;
	  height: 500rpx;
	  background-color: skyblue;
  }
</style>
