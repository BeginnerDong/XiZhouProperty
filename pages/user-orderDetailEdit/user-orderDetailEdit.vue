<template>
	<view>
		
		<view class="">
			<view class="item bg-white px-3 py-3 rounded10 font-24">
				<view class="font-26">{{mainData.description}}</view>
				<view class="imgList d-flex a-start flex-wrap">
					<view class="lis" v-for="(item,index) in submitData.mainImg" :key="index">
						<view class="closebtn" @click="deleteThis(index)">×</view><image :src="item.url" mode=""></image>
					</view>
					<view class="imgList">
						<view class="lis" @click="upLoadImg('mainImg')"><image src="../../static/images/probleml-icon.png" mode=""></image></view>
					</view>
				</view>
				
			</view>
			
			<view class="f5Bj-H20"></view>
			<view class="p-3 bg-white font-24">
				<view class="d-flex a-center">
					<image class="gpsIcon" src="../../static/images/taskl-icon.png" mode=""></image>
					<view class="ml-1">{{item.placeName.name?item.placeName.name:'无'}}</view>
				</view>
				<view class="d-flex a-center flex-wrap mt-2">
					<view>处理人：{{item.dealName.name?item.dealName.name:'无'}}</view>
					<view class="ml-5">负责人：{{item.chargeName.name?item.chargeName.name:'无'}}</view>
				</view>
			</view>
			
			<view class="f5Bj-H20"></view>
			
			<view class="submitbtn" style="margin-top: 100rpx;">
				<button class="btn" type="default" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">提交</button>
			</view>
			
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				submitData:{
					mainImg:[]
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			deleteThis(index){
				const self = this;
				self.submitData.mainImg.splice(index, 1);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
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
					sourceType:['camera'],
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
				postData.data = {
					mainImg:self.submitData.mainImg
				};
				postData.searchItem = {
					id:self.mainData.id
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功', 'none', 1000)
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				postData.getAfter = {
					dealName: {
						tableName: 'UserInfo',
						middleKey: 'deal_no',
						key: 'user_no',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['name']
					},
					chargeName: {
						tableName: 'UserInfo',
						middleKey: 'charge_no',
						key: 'user_no',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['name']
					},
					createName: {
						tableName: 'UserInfo',
						middleKey: 'user_no',
						key: 'user_no',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['name']
					},
					placeName: {
						tableName: 'Place',
						middleKey: 'relation_id',
						key: 'id',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['name']
					},
					typeName: {
						tableName: 'Label',
						middleKey: 'type',
						key: 'id',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['title']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.mainImg = self.mainData.mainImg;
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/order.css";
	
	.imgList .lis{width: 210rpx;height: 210rpx; position: relative;}
	.closebtn{z-index: 22;background-color: #000;border-radius: 50%;width: 36rpx;height: 36rpx;color: #fff;display: flex;justify-content: center; align-items: center;opacity: 0.5;font-size: 36rpx;top: -14rpx;right: -14rpx;}
</style>
