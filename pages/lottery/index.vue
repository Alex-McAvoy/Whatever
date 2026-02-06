<!--
 * @Description   摸奖页
 * @Author        Alex_McAvoy
 * @Date          2026-02-06 22:56:57
-->
<template>
	<view class="main">
		<!-- 导航栏 -->
		<mine-navbar :title="navbarTitle"></mine-navbar>
		<!-- 容器 -->
		<view class="container">
			<!-- 背景 -->
			<view class="bg">
				<!-- 标题容器 -->
				<view class="title-container">
					<!-- 标题 -->
					<view class="title">吃啥？</view>
				</view>
								
				<!-- 分隔 -->
				<image class="divide" :src="divideImgSrc" mode="widthFix"></image>

				<!-- 摸奖容器 -->
				<view class="lottery-container">
					<!-- 转盘容器 -->
					<view class="turntable-container">
						<view>TODO：转盘</view>
						<view>TODO：按钮</view>
					</view>
				</view>
				
			</view>
		</view>
		<!-- 底部导航栏 -->
		<mine-tabbar :current="-1"></mine-tabbar>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 导航栏title
				navbarTitle: "",
				// 横戟分隔图
				divideImgSrc: "/static/images/common/divide.png",
			};
		},
		onLoad(option) {
			// 获取索引
			let index = option.index
			if (index == 0) {
				// 修改导航栏
				this.navbarTitle = "日常摸奖"
			} else if (index == 1) {
				// 修改导航栏
				this.navbarTitle = "每周一摸"
			}
		}
	}
</script>

<style lang="scss" scoped>
/* 图鉴 */
.container {
	display: flex;
	flex-direction: column;

	/* 背景 */
	.bg {
		background-color: blanchedalmond;
		background-image: url("@/static/images/common/list_bg.png");
		background-position: center;
		background-repeat: no-repeat;
		background-attachment: fixed;
		background-size: cover;
		padding: 20px;
		margin: 30rpx;
		box-shadow: 3px -3px 5px rgba(0, 0, 0, 0.5);
		border-radius: 10px;
		border-style: groove;
		border-color: #413430;

		/* 标题容器 */
		.title-container {
			display: flex;
			flex-direction: row;
			justify-content: center;
			align-items: center;

			/* 标题 */
			.title {
				font-family: huaWenXingKai;
				color: black;
				font-size: 32px;
				font-weight: bold;
				text-align: center;
			}
		}

		/* 分隔 */
		.divide {
			width: 100%;
			height: auto;
			align-self: center;
		}

		/* 摸奖容器 */
		.lottery-container {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			/* 转盘容器 */
			.turntable-container {
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
			}
		}
	}
}
</style>