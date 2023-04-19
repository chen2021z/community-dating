<template>
	<view>
		<scroll-view scroll-x class="scroll-row border-white" :scroll-into-view="scrollInto" scroll-with-animation
			style="height:100rpx">
			<view v-for="(item, index) in tabBars" :key="index" :id="'tab'+index"
				class="scroll-row-item px-3 py-2 font-md" @click=changeTab(index)
				:class="tabIndex === index? 'text-main font-lg font-weight-bold':''">
				{{item.name}}
			</view>
		</scroll-view>


		<swiper :duration="150" :current="tabIndex" @change=onChangeTab :style="'height:'+scrollHeight+'px;'">
			<swiper-item v-for="(item, index) in newsList" :key="index">
				<scroll-view scroll-y="true" @scrolltolower=loadmore(index) :style="'height:'+scrollHeight+'px;'">
					<template v-if=item.list.length>

						<block v-for="(item2,index2) in item.list" :key="index2">
							<!-- 列表样式 -->
							<comonList :item="item2" :index="index2" @follow="follow" @doSupport="doSupport">
							</comonList>
							<!-- 全局分割线 -->
							<divider></divider>
						</block>

						<!-- 上拉加载 -->
						<load-more :loadmore="item.loadmore"></load-more>

					</template>
					<!-- 无数据情况 -->
					<template v-else>
						<nothing></nothing>
					</template>
				</scroll-view>
			</swiper-item>

		</swiper>
	</view>
</template>

<script>
	const demo = [{
		username: "昵称",
		userpic: "/static/default.jpg",
		newstime: "2023-4-14 下午17:00",
		isFollow: false,
		title: "标题",
		titlepic: "/static/demo/datapic/11.jpg",
		support: {
			type: "",
			support_count: 1,
			unsupport_count: 2,
		},
		comment_count: 2,
		share_num: 4
	}, {
		username: "昵称",
		userpic: "/static/default.jpg",
		newstime: "2023-4-14 下午17:00",
		isFollow: false,
		title: "标题",
		titlepic: "",
		support: {
			type: "support",
			support_count: 1,
			unsupport_count: 2,
		},
		comment_count: 2,
		share_num: 4
	}, {
		username: "昵称",
		userpic: "/static/default.jpg",
		newstime: "2023-4-14 下午17:00",
		isFollow: false,
		title: "标题",
		titlepic: "/static/demo/datapic/11.jpg",
		support: {
			type: "",
			support_count: 1,
			unsupport_count: 2,
		},
		comment_count: 2,
		share_num: 4
	}]
	import comonList from '@/components/common/common-list.vue'
	import loadMore from '@/components/common/load-more.vue'
	export default {
		components: {
			comonList,
			loadMore
		},
		data() {
			return {
				// 屏幕高度
				scrollHeight: 600,
				newsList: [],
				tabIndex: 0,
				// 顶部选项卡
				scrollInto: '',
				tabBars: [{
						name: '关注',
					},
					{
						name: '推荐',
					},
					{
						name: '体育',
					},
					{
						name: '热点',
					},
					{
						name: '财经',
					},
					{
						name: '娱乐',
					},
					{
						name: '军事',
					},
					{
						name: '历史',
					},
					{
						name: '本地',
					},
				],

			}
		},
		// 监听点击原生导航栏搜索框
		onNavigationBarSearchInputClicked() {
			uni.navigateTo({
				url: '../search/search',
				success: res => {},
				fail: () => {},
				complete: () => {}
			});
		},
		onLoad() {
			uni.getSystemInfo({
				success: res => {
					// 单位是px
					this.scrollHeight = res.windowHeight - uni.upx2px(100) - -res.statusBarHeight
				}
			})
			this.getData()
		},
		// 监听导航按钮点击事件
		onNavigationBarButtonTap() {
			uni.navigateTo({
				url: '../add-input/add-input'
			})
		},
		methods: {
			follow(e) {
				this.list[e].isFollow = true
				uni.showToast({
					title: '关注成功'
				})
			},
			changeTab(index) {
				if (this.tabIndex == index) return;
				this.tabIndex = index
				this.scrollInto = 'tab' + index
			},
			// swiper切换
			onChangeTab(current, source) {
				this.changeTab(current.detail.current)
			},
			//初始化列表数据
			getData() {
				let arr = []
				for (let i = 0; i < this.tabBars.length; i++) {
					// 生成列表模板
					let obj = {
						list: [],
						// 三种状态：上拉加载更多  加载中...  没有更多了
						loadmore: "上拉加载更多"
					}
					if (i < 2) {
						obj.list = demo
					}
					arr.push(obj)
				}
				this.newsList = arr
			},
			// 触底事件，加载更多
			loadmore(index) {
				let item = this.newsList[index]
				if (item.loadmore !== '上拉加载更多') return
				item.loadmore = '加载中...'
				// 模拟数据请求
				setTimeout(() => {
					item.list = [...item.list, ...item.list]
					item.loadmore = '上拉加载更多'
				}, 2000)
			},
			doSupport() {

			}




		}
	}
</script>

<style>



</style>