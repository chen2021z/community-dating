<template>
	<view>
		<uni-nav-bar :border="false" :fixed="true" :status-bar="true" @clickRight="clickRight">
			<view class="w-100 flex justify-center align-center">

				<!-- <view class="font-lg text-main mx-1 font-weight-bold" v-for="item,index in tabBars" : key="index">
					
				</view> -->
				<view class=" mx-1" v-for="(item,index) in tabBars" :key="index"
					:class="tabIndex === index ? 'font-lg text-main font-weight-bold' : 'font-md text-light-muted'"
					@click="changeTabIdx(index)">
					{{item.name}}
				</view>
			</view>

			<text class="iconfont icon-fatie_icon" slot="right" ></text>
		</uni-nav-bar>
		
		
		
		
		<swiper :duration="150" :current="tabIndex" @change=onChangeTab :style="'height:'+scrollHeight+'px;'">
			<swiper-item >
				<scroll-view scroll-y="true"  :style="'height:'+scrollHeight+'px;'" @scrolltolower="loadMoreEvent">
					<block v-for="(item,index) in demo" :key="index">
						<!-- 列表样式 -->
						<commonList :item="item" :index="index"  @doSupport="doSupport">
						</commonList>
						<!-- 全局分割线 -->
						<divider></divider>
					</block>
				</scroll-view>
			</swiper-item>  
			<swiper-item >
				<scroll-view scroll-y="true"  :style="'height:'+scrollHeight+'px;'">
					话题
				</scroll-view>
			</swiper-item>  
					
		
		</swiper>
	</view>
</template>

<script>
	import commonList from "@/components/common/common-list.vue"
	export default {
		components:{
			commonList
		},
		data() {
			return {
				tabBars: [{
						name: '关注'
					},
					{
						name: '话题'
					}
				],
				tabIndex: 0,
				// 屏幕高度
				scrollHeight: 600,
				demo : [
					{
					username: "昵称",
					userpic: "/static/default.jpg",
					newstime: "2023-4-14 下午17:00",
					isFollow: true,
					title: "标题",
					titlepic: "/static/demo/datapic/11.jpg",
					support: {
						type: "",
						support_count: 1,
						unsupport_count: 2,
					},
					comment_count: 2,
					share_num: 4
				},{
					username: "昵称",
					userpic: "/static/default.jpg",
					newstime: "2023-4-14 下午17:00",
					isFollow: true,
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
					isFollow: true,
					title: "标题",
					titlepic: "/static/demo/datapic/11.jpg",
					support: {
						type: "",
						support_count: 1,
						unsupport_count: 2,
					},
					comment_count: 2,
					share_num: 4
				}],
				loadmore: '上拉加载更多'
			}
		},
		onLoad() {
			uni.getSystemInfo({
				success: res => {
					// 单位是px
					this.scrollHeight = res.windowHeight - 44 - res.statusBarHeight
				}
			})
		},
		methods: {
			changeTabIdx(idx) {
				this.tabIndex = idx
			},
			// 点击右侧进入发布页
			clickRight(){
				uni.navigateTo({
					url:"/pages/add-input/add-input"
				})
			},
			onChangeTab(current, source) {
				this.changeTab(current.detail.current)
			},
			changeTab(index) {
				if (this.tabIndex == index) return;
				this.tabIndex = index
				// this.scrollInto = 'tab' + index
			},
			doSupport({type, index}){
				console.log(type, index);
			},
			// 下拉加载更多
			loadMoreEvent(){
				// 验证当前是否处于加载状态
				 if(this.loadmore !== '上拉加载更多') return;
				 this.loadmore = '加载中...'
				 // 模拟请求数据
				 setTimeout(()=>{
					 this.demo = [...this.demo, ...this.demo]
					 this.loadmore = '上拉加载更多'
				 },1000)
			}
		}
	}
</script>

<style>

</style>