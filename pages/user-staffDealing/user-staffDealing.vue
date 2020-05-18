<template>
	<view>

		<view class="px-3 py-3">
			<view>
				<textarea value="" placeholder="输入处理结果" placeholder-class="placeholder" />
			</view>
			<view class="picList d-flex a-start flex-wrap">
				<block v-for="(image,index) in imageList" :key="index">
					<view class="upImgLis">
						<image :src="image" :data-src="image" @tap="previewImage"></image>
					</view>
				</block>
				<view class="upBtn mt-2" @click="chooseImage"><image src="../../static/images/probleml-icon.png" mode=""></image></view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="default">提交</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				imageList: []
			}
		},
		onUnload() {
			this.imageList = []
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			chooseImage: async function() {
				uni.chooseImage({
					success: (res) => {
						this.imageList = this.imageList.concat(res.tempFilePaths);
					}
				})
			},
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		},
	};
</script>

<style>
	textarea{width: 100%;height: 260rpx;box-sizing: border-box;}
	.upBtn{width: 120rpx;height: 120rpx;}
	.upImgLis{width: 150rpx;height: 150rpx; margin: 0rpx 30rpx 30rpx 0;overflow: hidden;}
	.upImgLis:nth-of-type(4n){margin-right: 0;}
</style>
