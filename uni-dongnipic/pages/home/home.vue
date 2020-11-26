<template>
	<view>
		<view class="home_tab">
			<view class="home_tab_title">
				<view class="title_inner">
					<uni-segmented-control :current="current" :values="items.map(v=>v.title)" @clickItem="onClickItem" style-type="text"
					 active-color="#d4237a"></uni-segmented-control>
				</view>
				<view class="iconfont icon-search"></view>
			</view>
			<view class="home_tab_content">
				<view v-if="current === 0">
					<home-recommends></home-recommends>
				</view>
				<view v-if="current === 1">
					<home-category></home-category>
				</view>
				<view v-if="current === 2">
					<home-new></home-new>
				</view>
				<view v-if="current === 3">
					<home-album></home-album>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import HomeRecommends from '../components/Home/home-recommends.vue';
	import HomeCategory from '../components/Home/home-category.vue';
	import HomeNew from '../components/Home/home-new.vue';
	import HomeAlbum from "../components/Home/home-album.vue"
	export default {
		data() {
			return {
				items: [{
						title: "推荐"
					},
					{
						title: "娱乐"
					},
					{
						title: "最新"
					},
					{
						title: "热门"
					},
					{
						title: "分类"
					}
				],
				current: 0
			};
		},
		components: {
			HomeRecommends,
			HomeCategory,
			HomeNew,
			HomeAlbum
		},
		methods: {
			onClickItem(e) {
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex;
				}
			}
		}
	}
</script>

<style lang="scss">
	.home_tab {
		.home_tab_title {
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

		.home_tab_content {}
	}
</style>
