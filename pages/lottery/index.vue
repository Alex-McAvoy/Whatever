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
					<!-- 奖品列表wrapper -->
					 <view class="prize-list-wrapper">
						<!-- 奖品列表 -->
						<view class="prize-list">
						<!-- <view class="prize-list" :style="{ transform: `translateY(${translateY}px)`, transition: transitionStyle }"> -->
							<!-- 奖品项 -->
							<view class="prize-item" v-for="(item, index) in displayList" :key="index" :style="getItemStyle(index)">
								{{ item }}
							</view>
						</view>
					</view>
					<!-- 按钮 -->
					<view class="lottery-button" @click="startLottery"><font>摸一摸</font></view>
				</view>
			</view>
		</view>
		<!-- 底部导航栏 -->
		<mine-tabbar :current="-1"></mine-tabbar>
	</view>
</template>

<script>
import { created } from 'uview-ui/libs/mixin/mixin';

	export default {
		data() {
			return {
				// 导航栏title
				navbarTitle: "",
				// 分隔图
				divideImgSrc: "/static/images/common/divide.png",
				// 奖品列表
				prizeList: [],
				// 可见奖品数量
				visibleCount: 9,
				// 当前显示的奖品
				displayList: [],
				// 中间元素索引
				centerIndex: 4,
				// 是否正在滚动
				rotating: false,
				// 当前prizeList的起始索引
				startIndex: 0,
				// 是否有数据
				isData: true
			};
		},
		onLoad(option) {
			// 修改导航栏
			let index = option.index;
			if (index == 0) {
				this.navbarTitle = "日常摸奖";
			} else if (index == 1) {
				this.navbarTitle = "每周一摸";
			}
			// 读取数据
			const storageKey = index == 0 ? "daily" : "weekly";
			const data = uni.getStorageSync(storageKey)
			if (!data) {
				this.isData = false;
				return uni.showToast({
					"title": "无数据，请添加！",
					icon: "none"
				});
			}
			this.isData = true;
			this.prizeList = data.map(item => item.name);
			// 初始化displayList
			this.updateDisplayList()
		},
		created() {
		},
		methods: {
			updateDisplayList() {
				// 仅展示visibleCount个
				const newList = [];
				for (let i = 0; i < this.visibleCount; i++) {
					const prizeIndex = (this.startIndex + i) % this.prizeList.length;
					newList.push(this.prizeList[prizeIndex]);
				}
				this.displayList = newList;
			},
			getItemStyle(index) {
				// 当前索引与中间高亮索引的差值
				const diff = Math.abs(index - this.centerIndex);
				// 选中字体加粗
				const fontWeight = diff === 0 ? "bold" : "normal";
				// 字号大小
				const fontSizes = [72,63,54,45,36];
				// 间距
				const margins = [25, 20, 15, 10,5];
				// 透明度
				const opacity = 1 - diff * 0.2;
				return {
					fontSize: `${fontSizes[diff]}rpx`,
					color: `rgba(0, 0, 0, ${opacity})`,
					fontWeight: fontWeight,
					marginTop: `${margins[diff]}rpx`,
					marginBottom: `${margins[diff]}rpx`,
				};
			},
			startLottery() {
				// 若无数据，点击失效
				if (!this.isData) {
					return uni.showToast({
						"title": "无数据，请添加！",
						icon: "none"
					});
				}
				// 若正在滚动，点击失效
				if (this.rotating) return;
				this.rotating = true;

				// 总滚动时间 2s
				const totalTime = 2000;
				// 每步间隔 100ms
				const intervalTime = 100;
				// 总步数
				const steps = Math.floor(totalTime / intervalTime);

				// 随机选定最终奖品
				const targetIndex = Math.floor(Math.random() * this.prizeList.length);
				// 计算每步增加的索引
				const currentStart = this.startIndex;
				const totalShift = (targetIndex - this.centerIndex - currentStart + this.prizeList.length) % this.prizeList.length + this.prizeList.length * 3;
				const stepIncrement = totalShift / steps;
				
				// 步数计数器
				let stepCount = 0;
				// 用小数控制平滑滚动
				let floatIndex = this.startIndex;

				const interval = setInterval(() => {
					// 更新displayList
					floatIndex += stepIncrement;
					this.startIndex = Math.floor(floatIndex) % this.prizeList.length;
					this.updateDisplayList();

					// 计数器超出滚动次数，终止
					stepCount++;
					if (stepCount >= steps) {
						clearInterval(interval);
						this.rotating = false;
						// 修正startIndex，确保中间元素是最终目标
						this.startIndex = (targetIndex - this.centerIndex + this.prizeList.length) % this.prizeList.length;
						// 更新displayList
						this.updateDisplayList();

						// 输出结果
						const finalPrize = this.prizeList[targetIndex];
						uni.showToast({
							title: `吃 ${finalPrize}`,
							icon: "none"
						})
					}
				}, intervalTime);
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

			/* 标题容器 */
			.title-container {
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				/* 标题 */
				.title {
					font-family: huaWenXingKai;
					color: black;
					font-size: 32px;
					font-weight: bold;
					text-align: center;
					margin-bottom: 5%;
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
				flex: 1;

				/* 奖品列表wrapper */
				.prize-list-wrapper {
					display: flex;
					flex-direction: column;
					align-items: center;

					/* 奖品列表 */
					.prize-list {
						display: flex;
						flex-direction: column;
						align-items: center;

						/* 奖品项 */
						.prize-item {
							width: auto;
							text-align: center;
							transition: all 0.3s;
						}
					}
				}
				
				/* 抽奖按钮 */
				.lottery-button {
					display: flex;
					flex-direction: column;
					justify-content: center;
					align-items: center;
					
					background-image: url("@/static/images/common/lottery_button.png");
					background-repeat: round;
					width: 100px;
					padding: 5px 0px;
					margin-top: 100rpx;

					font {
						font-family: heiTi;
						font-weight: bold;
						font-size: 15;
						text-align: center;
						-webkit-background-clip: text;
						-webkit-text-fill-color: transparent;
						textShadow: 0px 0px 7px rgba(52, 255, 204, 0.1);
						background-image: -webkit-linear-gradient(top, #aca08d,#f3f1f4,#b9b196);	
						margin: 10rpx;
					}
				}
			}
		}
	}
}
</style>