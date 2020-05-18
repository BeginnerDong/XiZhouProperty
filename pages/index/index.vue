<template>
	<view>
		
		<view class="xqInfor mx-3">
			<view class="font-36 font-weight text-center pt-4 pb-3">西周物业</view>
			<view class="cont">
				<view>速度快和个人防护施工技术规划局发货的顾客发动机盖日工行卡拿过来是符合大家可舒服了回房间的思考和繁荣街事儿涸鲋得水尽快发货克里斯多夫GV回来是恢复健康的说法幅度飞机客人都湿了刚和艰苦奋斗，个刚恢复健康大哥恢复健康的两个刚和艰苦奋斗很给力股，积分的快感回复附近的开始。</view>
				<view class="">
					<image class="w" src="../../static/images/home-img.png" mode="widthFix"></image>
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
				is_show: false,
				wx_info:{},
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom: 140rpx;}	
	
	
</style>
