<!--
 * @Description   摸奖页
 * @Author        Alex_McAvoy
 * @Date          2026-02-06 22:56:57
-->
<template>
	<view class="main">
		<!-- 导航栏 -->
		<mine-navbar :title="navbarTitle"></mine-navbar>
		TODO：一个转盘，点一下转
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
</style>