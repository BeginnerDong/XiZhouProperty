<template>
	<view v-if="showAll">
		
		<view class="userHead  main-bg-color"></view>
		
		<view class="mx-3" style="margin-top: -150rpx;">
			<view class="userBox2 rounded10 bg-white font-24 px-3 d-flex a-center j-center shadow">
				<view class="text-center">
					<view class="userPhoto" style="overflow: hidden;">
						<image :src="userData.headImgUrl?userData.headImgUrl:'../../static/images/head.png'"></image>
					</view>
					<view class="font-30 mt-2" v-if="userData.nickname">{{userData.nickname?userData.nickname:''}}</view>
					<button style="margin-top: 10px;padding: 0 10px;font-size: 13px;background-color: #7a8eff;color: #fff;height: 25px;line-height: 25px;" v-else open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">点击登录</button>
				</view>
			</view>
			
		</view>
		
		<view class="myRowBetween font-26 mx-3 mt-3" v-if="userData.nickname">
			<view class="item d-flex a-center j-sb" v-if="userData.info.behavior==0" @click="Router.navigateTo({route:{path:'/pages/user-problem/user-problem'}})" >
				<view class="ll d-flex a-center">
					<image class="icon" src="../../static/images/about-icon.png" mode=""></image>
					<view class="">问题列表</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/icon.png" mode=""></image></view>
			</view>
			<view class="item d-flex a-center j-sb" v-if="userData.info.behavior==3" @click="Router.navigateTo({route:{path:'/pages/user-complaint/user-complaint'}})" >
				<view class="ll d-flex a-center">
					<image class="icon" src="../../static/images/about-icon1.png" mode=""></image>
					<view class="">投诉或建议</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/icon.png" mode=""></image></view>
			</view>
			<view class="item d-flex a-center j-sb" v-if="userData.info.behavior==2||userData.info.behavior==3" 
			@click="Router.navigateTo({route:{path:'/pages/user-inspect/user-inspect?type='+userData.info.behavior}})" >
				<view class="ll d-flex a-center">
					<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
					<view class="">巡检</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/icon.png" mode=""></image></view>
			</view>
			<view class="item d-flex a-center j-sb" v-if="userData.info.behavior!=0" 
			@click="Router.navigateTo({route:{path:'/pages/user-Task/user-Task?type='+userData.info.behavior}})" >
				<view class="ll d-flex a-center">
					<image class="icon" src="../../static/images/about-icon3.png" mode=""></image>
					<view class="">任务</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/icon.png" mode=""></image></view>
			</view>
			<view class="item d-flex a-center j-sb" v-if="userData.info.behavior==3||userData.info.behavior==1" 
			@click="Router.navigateTo({route:{path:'/pages/user-problem/user-problem'}})" >
				<view class="ll d-flex a-center">
					<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
					<view class="">问题提交</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/icon.png" mode=""></image></view>
			</view>
		</view>
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item Middle" @click="scanCode()" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">我的</view>
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
				userData:{},
				Utils:this.$Utils,
				showAll:false
			}
		},
		
		onLoad() {
			const self = this;
			if(uni.getStorageSync('user_token')){
				self.$Utils.loadAll(['getUserData'], self);
			}else{
				self.showAll = true
			}
		},
		
		methods: {
			
			showToast(){
				uni.showModal({
					title:'提示',
					content:'请先登录',
					showCancel:false
				})
			},
			
			scanCode() {
				const self = this;
				if(!uni.getStorageSync('user_token')){
					uni.showModal({
						title:'提示',
						content:'请先登录',
						showCancel:false
					})
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
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const callback = (user, res) => {
					console.log(res)
					self.getUserData();
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||wx.getStorageSync('user_info').headImgUrl==''||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0]
					};
					self.showAll = true;
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 140rpx;}
	
	.userHead{height: 200rpx;}
	
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
	.userPhoto{width: 130rpx;height: 130rpx;border-radius: 50%;overflow: hidden;margin: 0 auto;}
	
	
	.userBox2{width: 100%;height: 286rpx;position: relative; z-index: 1; }
	.userBox2 .item{flex: 1;}
	.userBox2 .item .icon{width: 48rpx;height:48rpx;display: block;margin: 0 auto;margin-bottom: 20rpx;position: relative;}
	.userBox2 .item .icon image{width: 100%;height: 100%;}

</style>
