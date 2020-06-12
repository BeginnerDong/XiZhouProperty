<template>
	<view>

		<view class="px-3 py-3">
			<view>
				<textarea v-model="content" placeholder="输入处理结果" placeholder-class="placeholder" />
			</view>
			
			<view class="picList d-flex a-start flex-wrap">
				<block v-for="(item,index) in bannerImg" :key="index">
					<view class="upImgLis">
						<image :src="item.url"  @click="previewImage(index)"></image>
					</view>
				</block>
				<view class="upBtn mt-2" @click="upLoadImg('bannerImg')"><image src="../../static/images/probleml-icon.png" mode=""></image></view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="default" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">提交</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				imageList: [],
				content:'',
				bannerImg:[]
			}
		},
		
		
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.content==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入处理结果', 'none')
					return
				};
				const callback = (user, res) => {
					console.log(res)
					self.messageUpdate();
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			upLoadImg(type) {
				const self = this;			
				
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					sourceType:['camera','album'],
					
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						var file = res.tempFiles[0];
						var obj = res.tempFiles[0].path.lastIndexOf(".");
						var ext = res.tempFiles[0].path.substr(obj+1);
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'headImg'
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			messageUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||wx.getStorageSync('user_info').headImgUrl==''||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				postData.searchItem = {
					id:self.id,
					user_type:0
				};
				postData.data = {
					content:self.content,
					bannerImg:self.bannerImg,
					deal:1
				};
				
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('处理成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageUpdate(postData, callback);
			},
			
			
			
			
			
			previewImage(e) {
				const self = this;
				var current = self.bannerImg[index].url;
				var urls = [];
				for (var i = 0; i < self.bannerImg.length; i++) {
					urls.push(self.bannerImg[i].url)
				};
				uni.previewImage({
					current: current,
					urls: this.bannerImg
				})
			},
		},
	};
</script>

<style>
	textarea{width: 100%;height: 260rpx;box-sizing: border-box;}
	.upBtn{width: 120rpx;height: 120rpx;}
	.upImgLis{width: 150rpx;height: 150rpx; margin: 0rpx 30rpx 30rpx 0;overflow: hidden;}
	.upImgLis:nth-of-type(4n){margin-right: 0;}
</style>
