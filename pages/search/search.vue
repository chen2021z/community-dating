<template>
	<view>
		<template v-if="searchList.length === 0">
			<view class="py-2 px-2 font-md">搜索历史</view>
			<view class="flex flex-wrap">
				<view class="border rounded font my-1 mx-2 px-1" v-for="item,index in list" :key="index"
					@click="clickSearchItem()">{{item}}</view>
			</view>
		</template>

		<template v-else>
			<block v-for="(item,index) in searchList" :key="index">
				<!-- 列表样式 -->
				<comonList :item="item" :index="index">
				</comonList>item
			</block>
		</template>
	</view>
</template>

<script>
	import comonList from '@/components/common/common-list.vue'
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
	export default {
		components: {
			comonList
		},
		data() {
			return {
				list: [
					'关键词1',
					'关键词2',
					'关键词3',
					'关键词4',
					'关键词5',
					'关键词6',
					'关键词7',
					'关键词8',
				],
				searchText: '',
				searchList: []
			}
		},
		// 监听原生标题栏搜索输入框输入内容变化事件
		onNavigationBarSearchInputChanged(e) {
			this.searchText = e.text
		},
		// 监听原生标题栏按钮点击事件，参数为Object	
		onNavigationBarButtonTap(e) {
			if (e.index === 0) {
				this.searchEvent()
			}
		},
		methods: {
			// 点击搜索input
			searchEvent() {
				// 收起键盘  
				uni.hideKeyboard()
				// 加载loading状态
				uni.showLoading({
					title: '加载 中',
					mask: false
				})

				setTimeout(() => {
					uni.hideLoading()
					this.searchList = demo

				}, 1000)
			},
			// 点击搜索历史item
			clickSearchItem(name) {
				
				this.searchText = name
				this.searchEvent()
			}
		}
	}
</script>

<style>

</style>