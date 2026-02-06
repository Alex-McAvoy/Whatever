<!--
 * @Description   摸奖页
 * @Author        Alex_McAvoy
 * @Date          2026-02-06 22:40:03
-->
<template>
	<view class="main">
		<u-sticky>
			<page-search></page-search>
		</u-sticky>
		<view class="container">
			<view class="images-container" v-for="item in images">
				<view class="image-container" @click="handleBuilding(item.index)">
					<image class="image" :src="item.imgSrc" mode="widthFix"></image>
					<view class="cover">
						{{item.title}}
					</view>
				</view>
			</view>
		</view>
		<mine-tabbar :current="0"></mine-tabbar>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				images: [{
					index: 0,
					imgSrc: "/static/images/common/archive/index_yiDong.png",
					title: "日常摸奖"
				}, {
					index: 1,
					imgSrc: "/static/images/common/archive/index_shiZhouNian.png",
					title: "每周一摸"
				}]
			}
		},
		onLoad() {
			// 隐藏原生tabbar
			uni.hideTabBar({})
		},
		methods: {
			handleBuilding(index) {
				if (index == 0) {
					uni.navigateTo({
						url: "/pages/lottery/index?index=" + index
					})
				} else if (index == 1) {
					uni.navigateTo({
						url: "/pages/lottery/index?index=" + index
					})
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		margin-top: 80px;

		.images-container {
			display: flex;
			flex-direction: column;
			align-items: center;
			margin: 40rpx 20rpx;

			.image-container {
				display: grid;
				grid-template-columns: 100%;
				grid-template-rows: 80% 20%;
				width: 100%;

				.image {
					grid-column: 1;
					grid-row: 1/3;
					width: 100%;
					box-shadow: 0 -3px 5px rgba(0, 0, 0, 0.5);
				}

				.cover {
					grid-column: 1;
					grid-row: 2;
					z-index: 1;
					padding: 10px;
					background-color: rgba(0, 0, 0, 0.5);

					color: #ffffff;
					font-size: 16px;
					text-align: center;
					justify-content: center;

				}
			}
		}

	}
</style>