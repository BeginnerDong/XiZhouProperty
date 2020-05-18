<template>
	<view>

		<view class="scanBox text-center">
			<view class="camera"></view>
			<view class="font13 text">将二维码放入框内，即可自动扫描</view>
			
			<!-- 以下扫描结果注释跳转按钮 可删除 -->
			<view class="d-flex a-center j-sb mx-5 mt-5">
				<view @click="Router.navigateTo({route:{path:'/pages/scanProblem/scanProblem'}})">扫描问题</view>
				<view @click="Router.navigateTo({route:{path:'/pages/scanDetail/scanDetail'}})">扫描信息</view>
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
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/navbar.css";
	page{background: #7e7e7e;}
	.scanBox{ position: fixed; top: 50%;left: 0;width: 100%; transform: translateY(-60%);}
	.scanBox .camera{ width: 400rpx; height: 400rpx; border: 2rpx solid #F3D012; background: #fff;margin: 0 auto;}
	.scanBox .text{color: #fff; font-size: 26rpx; margin-top: 20rpx;}
</style>
