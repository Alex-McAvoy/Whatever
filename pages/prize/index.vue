<!--
 * @Description   奖品添加页
 * @Author        Alex_McAvoy
 * @Date          2026-02-06 22:40:03
-->
<template>
	<view class="main">
		<page-search></page-search>
		<view class="container">
			<view class="bg">
				<view class="content-container">
					<view class="title">
						<text>奖池</text>
					</view>
					<image class="divide" :src="divideImgSrc" mode="widthFix"></image>
					<u-tabs :list="tabsList" class="tabs" lineColor="#60544a" lineWidth="50"
						:activeStyle="tabsActive" @click="getTabsList"></u-tabs>
					<view class="prize-container">
						<scroll-view scroll-y class="prize-scroll">
							<view v-if="!isData" class="empty">
								<text>暂无数据，请添加！</text>
							</view>
							<view v-for="item in prizeList" :key="item.id" class="prize-item">
								<text class="prize-name">
									{{ item.name }}
								</text>
								<view class="delete-btn" @click="deletePrize(item.id)">
									<text>删除</text>
								</view>
							</view>
						</scroll-view>
					</view>
				</view>
			</view>
			
		</view>
		<mine-tabbar :current="1"></mine-tabbar>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 分隔图
				divideImgSrc: "/static/images/common/divide.png",
				// 标签栏显示
				tabsList:  [{"name": "日常摸奖"},{"name": "每周一摸"}],
				// 标签栏选中样式
				tabsActive: {
					padding: "0 10rpx",
					color: "#1e1b1b",
					fontWeight: "bold",
					fontWeight: "600",
					transform: "scale(1.3)",
				},
				// 标签栏未选中样式
				tabsInactive: {
					padding: "10rpx",
					fontSize: "30rpx",
					fontWeight: "600",
				},
				// 标签索引
				tabsIndex: 0,
				// 是否有数据
				isData: true,
				// 奖品列表
				prizeList: []
			}
		},
		onLoad() {
			uni.hideTabBar({});
		},
		onShow() {
			this.getPrizeList();
		},
		methods: {
			getTabsList(item) {
				this.tabsIndex = item.index;
				this.getPrizeList();
			},
			getPrizeList() {
				// 读取数据
				const storageKey = this.tabsIndex == 0 ? "daily" : "weekly";
				const data = uni.getStorageSync(storageKey);
				// 为空不显示
				if (!data || data.length == 0) {
					this.isData = false;
					this.prizeList = [];
					return;
				}
				this.isData = true;
				this.prizeList = data;
			},
			deletePrize(id) {
				uni.showModal({
					title: '提示',
					content: '确定要删除吗？',
					cancelText: '取消',
					confirmText: '确定',
					success: (res) => {
						if(res.confirm) {
							// 读取数据
							const storageKey = this.tabsIndex == 0 ? "daily" : "weekly";
							let data = uni.getStorageSync(storageKey);
							// 过滤掉要删除的项
							data = data.filter(item => item.id != id);
							// 重新开始从1对每一项赋值
							data.forEach((item, index) => {
								item.id = index + 1;
							});
							// 保存
							uni.setStorageSync(storageKey, data);
							// 更新页面
							this.prizeList = data;
							// 更新空状态
							if (data.length == 0) {
								this.isData = false;
							}
							uni.showToast({
								title: "删除成功",
								icon: "success"
							});
						} else if (res.cancel) {
							uni.showToast({
								title: "取消成功",
								icon: "none"
							});
						}
					}
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
.main{
	display: flex;
	flex-direction: column;
	height: 100vh;

	/* 图鉴 */
	.container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		flex: 1;

		/* 背景 */
		.bg {
			display: flex;
			flex-direction: column;
			justify-content: center;
			flex:1;

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
		

			/* 内容容器 */
			.content-container {
				display: flex;
				flex-direction: column;
				flex: 1;

				margin-top: 10px;
				box-shadow: 0 -3px 5px rgba(0, 0, 0, 0.5);

				background-image: url("@/static/images/common/top.png"), url("@/static/images/common/bottom.png");
				background-position: top left, bottom right;
				background-repeat: no-repeat;
				background-color: #ffffff;
				padding: 20px;

				/* 标题 */
				.title {
					font-family: heiTi;
					color: black;
					font-size: 30px;
					font-weight: bold;
					padding: 20px 10px;
					text-align: center;
				}

				/* 分隔 */
				.divide {
					justify-self: center;
					align-self: center;
					width: 100%;
					height: auto;
				}

				/* 标签栏 */
				.tabs {
					height: 80rpx;
					padding: 5px 10px 0px 10px;
					font-family: huaWenXingKai;
					background-color: rgba(255, 255, 255, 0);
					margin-bottom: 20px;
				}
				::v-deep .u-tabs__wrapper__nav {
					width: 100%;
					display: flex;
					flex-direction: row;
					position: relative;
					justify-content: space-between;
				}
				::v-deep .u-tabs__wrapper__nav__item {
					flex: 1 1 0% !important;
				}

				/* 表单容器 */
				.prize-container {
					display: flex;
					flex-direction: column;
					flex: 1;
					gap: 20px;

					.prize-scroll {
						flex: 1;

						.empty {
							text-align: center;
							color: #999;
							padding-top: 100rpx;
							font-family: heiTi;
							font-size: 32rpx;
							font-weight: 600;
							letter-spacing: 2rpx;
						}

						.prize-item {
							display: flex;
							justify-content: space-between;
							align-items: center;
							padding: 20rpx;
							margin-bottom: 20rpx;
							background: rgba(250,250,250,0.5);
							border-radius: 10rpx;
							box-shadow: 0 4rpx 10rpx rgba(0,0,0,0.1);

							.prize-name {
								display: flex;
								flex: 1;
								justify-content: space-around;
								align-items: center;
								text-align: center;
								gap: 10rpx;
								color: #000;
							}

							.delete-btn {
								padding: 5rpx 10rpx;
								background-color: transparent;
								border: 1rpx solid #000;
								border-radius: 5rpx;
								font-size: 24rpx;
								color: #000;
								cursor: pointer;
								text-align: center;
							}
						}
					}
				}
			}
		}
	}
}
</style>