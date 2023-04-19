<template>
	<view>
		<!-- 自定义导航 -->
		<uni-nav-bar left-icon="back"  statusBar :border="false" :height="50" @clickLeft="clickLeft">
			 <view class="flex justify-center align-center uni-textarea font-md">所有人可见<text class="iconfont icon-shezhi"></text></view>
		</uni-nav-bar>
		<!-- 文本域 -->
		<textarea v-model="content" class=" w-100 px-2" placeholder="说一句话吧"></textarea>
		
		
		<upload-image @change="change" :list="imageList" ref="uploadImage" :showAddImg="showAddImg"></upload-image>
		
		<!-- 底部操作栏 -->
		<view class="fixed-bottom bg-white flex align-center" style="height: 85rpx; bottom: 20rpx;">
			<view class="iconfont icon-caidan footer-btn animate__animated" hover-class="animate__jello"></view>
			<view class="iconfont icon-huati footer-btn animate__animated" hover-class="animate__jello"></view>
			<view class="iconfont icon-tupian footer-btn animate__animated" hover-class="animate__jello" @click="clickIcon('uploadImage')"></view>
			<view class="bg-main text-white ml-auto flex justify-center align-center mr-2 rounded" style="width: 140rpx;height: 60rpx;">发送</view>
		</view> 
	</view>
</template> 

<script>
	import uploadImage from '@/components/common/upload-image.vue'
	export default {  
		components:{
			uploadImage
		},
		data() {
			return {
				content:'',
				imageList:[],
				// 是否已经显示保存弹窗
				showBack:false
			}
		},
		// 监听页面返回
		onBackPress() {
			// 弹窗只显示一次
			if((this.content !== '' || this.imageList.length > 0) && !this.showBack){
				uni.showModal({
					// title: '',
					content: '是否要保存为草稿？',
					showCancel: true,
					cancelText: '不保存',
					confirmText: '保存',
					success: res => {
						if(res.confirm){
							this.store()
						}else{
							// 清除缓存
							uni.removeStorage({
								key:'add-input'
							})
						}
						// 手动返回
						uni.navigateBack()
					},
				}); 
				this.showBack = true
				return true
			}
		},
		onLoad() {
			let data = null
			uni.getStorage({
				key:'add-input',
				success:(res)=> {
					if(res){ 
						data = JSON.parse(res.data)
						this.content = data.content
						this.imageList = data.imageList
					}
				}
			})
		},
		computed:{
			showAddImg(){
				return this.imageList.length > 0;
			}
		},
		methods: {
			// 导航栏返回
			clickLeft(){
				uni.navigateBack()
			},
			// 选中图片
			change(imageList){
				this.imageList = imageList
			},		
			store(){
				uni.setStorage({
					key:'add-input',
					data:JSON.stringify({
						content:this.content,
						imageList: this.imageList
					})
				})
			},
			clickIcon(refName){
				switch (refName){
					case 'uploadImage':
						this.$refs.uploadImage.chooseImage()
						break;
					default:
						break;
				}
			}
		}
	}
</script>

<style scoped>
	
.footer-btn{
	width:85rpx;
	height: 85rpx;
	display: flex;
	justify-content: center;
	align-items: center;
	font-size: 50rpx;
}
</style>
