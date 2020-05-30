<template>
	<view>
		
		<view class="userHead  main-bg-color"></view>
		
		<view class="mx-3" style="margin-top: -150rpx;">
			<view class="userBox2 rounded10 bg-white font-24 px-3 d-flex a-center j-center shadow">
				<view class="text-center">
					<view class="userPhoto" style="overflow: hidden;"><open-data type="userAvatarUrl"></open-data></view>
					<view class="font-30 mt-2"><open-data type="userNickName"></open-data></view>
				</view>
			</view>
		</view>
		
		<view class="myRowBetween font-26 mx-3 mt-3">
			<view class="item d-flex a-center j-sb" v-if="userInfoData.behavior==0" @click="Router.navigateTo({route:{path:'/pages/user-problem/user-problem'}})" >
				<view class="ll d-flex a-center">
					<image class="icon" src="../../static/images/about-icon.png" mode=""></image>
					<view class="">问题列表</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/icon.png" mode=""></image></view>
			</view>
			<view class="item d-flex a-center j-sb" v-if="userInfoData.behavior==3" @click="Router.navigateTo({route:{path:'/pages/user-complaint/user-complaint'}})" >
				<view class="ll d-flex a-center">
					<image class="icon" src="../../static/images/about-icon1.png" mode=""></image>
					<view class="">投诉或建议</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/icon.png" mode=""></image></view>
			</view>
			<view class="item d-flex a-center j-sb" v-if="userInfoData.behavior==2" @click="Router.navigateTo({route:{path:'/pages/user-inspect/user-inspect'}})" >
				<view class="ll d-flex a-center">
					<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
					<view class="">巡检</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/icon.png" mode=""></image></view>
			</view>
			<view class="item d-flex a-center j-sb" v-if="userInfoData.behavior==2||userInfoData.behavior==1" @click="Router.navigateTo({route:{path:'/pages/user-Task/user-Task'}})" >
				<view class="ll d-flex a-center">
					<image class="icon" src="../../static/images/about-icon3.png" mode=""></image>
					<view class="">任务</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/icon.png" mode=""></image></view>
			</view>
			<view class="item d-flex a-center j-sb" v-if="userInfoData.behavior==3||userInfoData.behavior==1" @click="Router.navigateTo({route:{path:'/pages/user-ProblemSubmit/user-ProblemSubmit'}})" >
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
			<view class="navbar_item Middle" @click="Router.redirectTo({route:{path:'/pages/scan/scan'}})" >
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
				userInfoData:{}
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		methods: {

			getUserInfoData() {
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
