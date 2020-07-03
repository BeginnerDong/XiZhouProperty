<template>
	<view>
		
		<view class="xqInfor mx-3">
			<view class="font-36 font-weight text-center pt-4 pb-3">{{mainData.title}}</view>
			<view class="cont">
				<view class="content ql-editor" style="padding:0;"
				v-html="mainData.content">
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item Middle" @click="scanCode()" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				mainData:{}
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				
				return {
					title:'西周物业',
					path: '/pages/index/index', //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}else{
				return {
					title:'西周物业',
					path: '/pages/index/index', //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}
		},
		
		methods: {
			
			scanCode() {
				const self = this;
				if(!uni.getStorageSync('user_token')){
					uni.showModal({
						title:'提示',
						content:'您还未登录，是否去登录？',
						success(res) {
							if(res.confirm){
								self.Router.redirectTo({
									route: {
										path: '/pages/user/user'
									}
								})
							}
						}
					});
					return
				};
				uni.scanCode({
					success: function(res) {
						/* self.Router.navigateTo({
							route: {
								path: '/pages/storeOrder-hexiao/storeOrder-hexiao?id=' + res.result
							}
						}) */
						console.log('条码内容：' + res.result);
						self.result = res.result.split('-');
						if(self.result[0]&&self.result[0]=='Article'){
							self.Router.navigateTo({route:{path:'/pages/scanDetail/scanDetail?id='+self.result[1]}})
						}else if(self.result[0]&&self.result[0]=='Place'){
							self.Router.navigateTo({route:{path:'/pages/scanProblem/scanProblem?id='+self.result[1]}})
						}else{
							self.$Utils.showToast('二维码无效')
						}
						
					}
				});
			},
			
			tokenGet() {
				const self = this;
				const postData = {
					searchItem: {
						user_no: 'U530127269146934'
					}
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.userData = res.info;
						uni.setStorageSync('user_token', res.token);
						uni.setStorageSync('user_no', res.info.user_no);
						uni.setStorageSync('user_info', res.info);
						var time = parseInt(new Date().getTime()) + 3500000;
						uni.setStorageSync('token_expire_time',time);
					}
					console.log('res', res)
					self.$Utils.finishFunc('tokenGet');
				};
				self.$apis.tokenGet(postData, callback);
			},
			
			/* getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||wx.getStorageSync('user_info').headImgUrl==''||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			}, */
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					id:1
				};
				/* postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['首页介绍']],
						},
						condition:'in'
					}
				}; */
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom: 140rpx;}	
	
	
</style>
