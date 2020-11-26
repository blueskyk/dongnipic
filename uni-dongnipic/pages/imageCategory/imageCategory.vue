<template>
	<view>
		<view class="category_tab">
			<view class="category_tab_title">
				<view class="title_inner">
					<uni-segmented-control :current="current" :values="items.map(v=>v.title)" @clickItem="onClickItem" style-type="text"
					 active-color="#d4237a"></uni-segmented-control>
				</view>
				<view class="iconfont icon-search"></view>
			</view>
			<scroll-view @scrolltolower="handleScrollLower" scroll-y enable-flex class="category_tab_content">
				<view class="cate_item" v-for="item in vertical" :key="item.id">
					<image :src="item.thumb" mode="widthFix"></image>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				items: [{
						title: "最新",
						order: "new"
					},
					{
						title: "热门",
						order: "hot"
					}
				],
				current: 0,
				// 请求需要携带的参数
				params: {
					limit: 30,
					skip: 0,
					order: "new"
				},
				id: 0,
				vertical: [],
				hasMore: true
			}
		},
		methods: {
			onClickItem(e) {
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex;
				} else {
					return;
				}
				this.params.order = this.items[e.currentIndex].order;
				this.params.skip = 0;
				this.vertical = []
				this.getList()
			},
			// 获取数据
			getList() {
				this.request({
					url: `http://157.122.54.189:9088/image/v1/vertical/category/${this.id}/vertical`,data: this.params
				}).then(res => {
					if(res.res.vertical.length === 0) {
						this.hasMore = false;
						uni.showToast({
							title: "没有更多数据了",
							icon: "none"
						});
						return;
					}
					this.vertical =[...this.vertical,...res.res.vertical] ;
				})
			},
			handleScrollLower() {
				if(this.hasMore) {
					// 设置跳过的条数
					this.params.skip += this.params.limit;
					this.getList();
				} else {
					uni.showToast({
						title:"没有更多数据了",
						icon: "none"
					});
				}
			}
		},
		onLoad(options) {
			uni.setNavigationBarTitle({
				title: "图片详情"
			});
			this.id = options.id;
			this.getList()
		}
	}
</script>

<style lang="scss">
	.category_tab {
		.category_tab_title {
			position: relative;

			.title_inner {
				width: 60%;
				margin: 0 auto;
			}

			.icon-search {
				position: absolute;
				top: 50%;
				right: 5%;
				transform: translateY(-50%);

			}
		}

		.category_tab_content {
			height: calc( 100vh - 36px);
			display: flex;
			flex-wrap: wrap;
			.cate_item {
				width: 33.33%;
				border: 5rpx solid #fff;
			}
		}
	}
</style>
