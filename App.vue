<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
			// 检测更新
			// 网络监听
			// 初始化数据 
		},
		onShow: function() {
			console.log('App Show')
			
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import url('common/uni.css');
	/* 自定义图标库 */
	@import url('./common/icon.css');
	/* 引入动画库 */
	@import url("animate.css");
	/* 自定义样式库 */
	@import "./common/free.css";
	/* 全局样式 */
	@import "./common/common.css";
</style> 
