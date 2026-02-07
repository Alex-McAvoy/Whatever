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
						<text>奖池扩充</text>
					</view>
					<image class="divide" :src="divideImgSrc" mode="widthFix"></image>
					<u-tabs :list="tabsList" class="tabs" lineColor="#60544a" lineWidth="50"
						:activeStyle="tabsActive" @click="getTabsList"></u-tabs>
					<view class="form-container">
						<view class="form-body">
							<u--form :model="formData" labelPosition="left" labelWidth="80">
								<u-form-item label="名称">
									<u--input v-model="formData.name" placeholder="请输入名称"></u--input>
								</u-form-item>
							</u--form>
						</view>
						<view class="form-footer">
							<u-button class="submit-btn" text="添加" @click="handleSubmit"></u-button>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		<mine-tabbar :current="2"></mine-tabbar>
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
				// 表单
				formData: {
					name: "",
				}
			}
		},
		onLoad() {
			uni.hideTabBar({});
		},
		methods: {
			getTabsList(item) {
				this.tabsIndex = item.index
			},
			handleSubmit() {
				// 基础校验
				const name = this.formData.name?.trim();
				if (!name) {
					uni.showToast({
						title: "禁止为空！",
						icon: "none"
					});
					return;
				}

				// 根据 tabsIndex 决定 key
				const storageKey = this.tabsIndex == 0 ? "daily" : "weekly";

				// 读取数据
				let oldData = uni.getStorageSync(storageKey);
				if (!oldData) {
					oldData = [];
				}

				if (oldData.some(item => item.name == name)){
					return uni.showToast({
						"title": "数据已存在！",
						icon: "none"
					});
				}

				// 计算自增id
				let newId = 1;
				if (oldData.length > 0) {
					const lastItem = oldData[oldData.length - 1];
					newId = (lastItem.id || 0) + 1;
				}

				// 新数据
				const newItem = {
					id: newId,
					name: name
				};

				// 追加
				oldData.push(newItem)
				
				// 保存
				uni.setStorageSync(storageKey, oldData)

				// 清空表单
				this.formData = {
					name: "",
				}
				uni.showToast({
					title: "添加成功",
					icon: "success"
				})
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
				// flex: 1;

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
				.form-container {
					display: flex;
					flex-direction: column;
					flex: 1;
					gap: 20px;

					.form-body {
						flex: 1;
						overflow-y: auto;
						padding: 20rpx 0;
					}

					.form-footer {
						padding-top: 20rpx;

						::v-deep .submit-btn {
							background-color: transparent !important;
							border: 2rpx solid #000 !important;
						}

						::v-deep .submit-btn .u-button__text {
							color: #000 !important;
							font-weight: 600;
						}

					}

					::v-deep .u-form-item__body__left__content__label {
						font-family: heiTi;
						font-size: 32rpx;
						font-weight: 600;
						color: #1e1b1b;
						letter-spacing: 2rpx;
					}

				}
			}
		}
	}
}
</style>