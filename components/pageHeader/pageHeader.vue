<!--
 * @Description   页面头部组件
 * @Author        Alex_McAvoy
 * @Date          2026-02-06 22:39:56
-->
<template>
	<view class="page-header">
		<view class="content">
			<view class="title">{{ title }}</view>
			<view v-if="subtitle" class="subtitle">{{ subtitle }}</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "pageHeader",
		props: {
			title: {
				type: String,
				default: "吃啥？随便"
			},
			subtitle: {
				type: String,
				default: ""
			}
		}
	}
</script>

<style lang="scss" scoped>
	.page-header {
		padding: 28rpx 40rpx 34rpx;
		background-image: linear-gradient(90deg, rgba(30, 27, 27, 0.72), rgba(30, 27, 27, 0.18)),
			url("@/static/images/search/bg.png");
		background-position: center center;
		background-size: cover;
		box-shadow: 0 6rpx 18rpx rgba(0, 0, 0, 0.18);

		.content {
			display: flex;
			flex-direction: column;
			gap: 10rpx;
		}

		.title {
			font-family: huaWenXingKai;
			font-size: 46rpx;
			font-weight: bold;
			color: #fff8e6;
			text-shadow: 0 4rpx 10rpx rgba(0, 0, 0, 0.42);
			line-height: 1.2;
		}

		.subtitle {
			font-family: heiTi;
			font-size: 24rpx;
			font-weight: 600;
			color: rgba(255, 248, 230, 0.86);
			line-height: 1.35;
			letter-spacing: 1rpx;
		}
	}
</style>
