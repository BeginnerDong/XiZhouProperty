<template>
	<view>

		<view class="editLine mx-3">
			<view class="item d-flex a-center j-sb" @click="Router.navigateTo({route:{path:'/pages//'}})" >
				<view class="ll">选择类型</view>
				<view class="rr d-flex a-center j-end" >
					<view class="d-flex a-center">
						
						<view class=" color9">
							<picker mode="selector" @change="chooseType" :range="labelData" range-key="title">
								{{labelData[index]?labelData[index].title:'请选择'}}
							</picker>
						</view>
						<image class="arrowR" src="../../static/images/icon.png" mode=""></image>
					</view>
					
				</view>
			</view>
			<view class="item">
				<view>
					<textarea v-model="submitData.description" placeholder="详情描述" placeholder-class="placeholder" />
				</view>
				
				<view class="picList d-flex a-start flex-wrap">
					<block v-for="(item,index) in submitData.mainImg" :key="index">
						<view class="upImgLis">
							<image :src="item.url"  @click="previewImage(index)"></image>
						</view>
					</block>
					<view class="upBtn mt-2" @click="upLoadImg('mainImg')"><image src="../../static/images/probleml-icon.png" mode=""></image></view>
				</view>
			</view>
			<view class="item  d-flex a-center j-sb">
				<view class="ll">是否匿名</view>
				<view class="rr d-flex a-center j-end">
					<view class="d-flex a-center mgr25" @click="setChange('1')">
						<image class="setIcon" :src="submitData.nameless==1?'../../static/images/probleml-icon3.png':'../../static/images/probleml-icon2.png'" mode=""></image>
						<view>是</view>
					</view>
					<view class="d-flex a-center ml-5" @click="setChange('0')">
						<image class="setIcon" :src="submitData.nameless==0?'../../static/images/probleml-icon3.png':'../../static/images/probleml-icon2.png'" mode=""></image>
						<view>否</view>
					</view>
				</view>
			</view>
			<view class="item  d-flex a-center j-sb" v-if="submitData.nameless==0">
				<view class="ll">联系电话</view>
				<view class="rr">
					<input type="number" v-model="submitData.phone" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		
		<!-- <view class="black-bj" v-show="is_show"></view>
		<view class=" typeShow rounded10 bg-white font-26 px-3" v-show="is_typeShow">
			<view class="item d-flex a-center j-sb" v-for="(item,index) in typeArray" :key="index" @click="typeChange(index)">
				<view>{{item}}</view>
				<view class="icon"><image :src="typeCurr==index?'../../static/images/probleml-icon1.png':''" mode=""></image></view>
			</view>
		</view> -->
		
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
				index:-1,
				submitData:{
					type:'',
					description:'',
					mainImg:'',
					class:'',
					behavior:1,
					relation_id:'',
					nameless:1,
					deal_no:'',
					charge_no:'',
					thirdapp_id:2,
					phone:''
				},
				labelData:[]
			}
		},
		
		
		onLoad(options) {
			const self = this;
			self.submitData.relation_id = options.id;
			self.$Utils.loadAll(['getLabelData','getUserInfoData'], self);
		},
		
		methods: {
			
			chooseType(e){
				const self = this;
				self.index = e.detail.value,
				self.submitData.type = self.labelData[self.index].id;
				self.getRelationData()
			},
			
			getRelationData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					relation_two:self.submitData.relation_id,
					type:self.submitData.type
				};
				postData.getAfter = {
					userInfo:{
						tableName:'UserInfo',
						middleKey:'relation_one',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.relationData = res.info.data[0];
						if(self.relationData.userInfo&&self.relationData.userInfo[0]){
							self.submitData.deal_no = self.relationData.userInfo[0].user_no;
							self.submitData.charge_no = self.relationData.userInfo[0].parent_no;
						}
					}
				};
				self.$apis.relationGet(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||wx.getStorageSync('user_info').headImgUrl==''||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						self.submitData.class=self.userInfoData.behavior+1
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.mainImg;
				if(self.submitData.nameless==1){
					delete newObject.phone;
				};
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					const callback = (user, res) => {
						console.log(res)
						self.messageAdd();
					};
					self.$Utils.getAuthSetting(callback);
						
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
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
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||wx.getStorageSync('user_info').headImgUrl==''||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
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
				self.$apis.messageAdd(postData, callback);
			},
			
			
			
			
			
			previewImage(e) {
				const self = this;
				var current = self.submitData.mainImg[index].url;
				var urls = [];
				for (var i = 0; i < self.submitData.mainImg.length; i++) {
					urls.push(self.submitData.mainImg[i].url)
				};
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
			
			setChange(seltCurr){
				const self = this;
				if(seltCurr!=self.submitData.nameless){
					self.submitData.nameless = seltCurr;
				}
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'parentid',
						key:'id',
						searchItem:{
							title: ['in', ['问题类别']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data;
					};
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.upBtn{width: 120rpx;height: 120rpx;}
	.upImgLis{width: 150rpx;height: 150rpx; margin: 0rpx 30rpx 30rpx 0;overflow: hidden;}
	.upImgLis:nth-of-type(4n){margin-right: 0;}
	.setIcon{width: 32rpx;height: 32rpx;display: block;margin-right: 10rpx;}
	.rr input{text-align: right;}
	
	.typeShow{width: 80%;position: fixed; top: 50%;left: 50%;z-index: 50;transform: translate(-50%,-50%);}
	.typeShow .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.typeShow .item:last-child{border-bottom: 0;}
	.typeShow .item .icon{width: 28rpx;height: 22rpx;}
</style>
