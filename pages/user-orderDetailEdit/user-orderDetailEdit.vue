<template>
	<view>
		
		<view class="">
			<view class="item bg-white px-3 py-3 rounded10 font-24">
				<view class="font-26">凤凰健康的首付款如何法规的数据库官方回复空间大使馆副高房价的合格刚恢复健康的航空凤凰健康的首付款如何法规的数据库官方回复空间大使馆副高房价的合格刚恢复健康的航空GV发</view>
				<view class="imgList d-flex a-start flex-wrap">
					<view class="lis" v-for="(item,index) in imgData" :key="index"><view class="closebtn">×</view><image src="../../static/images/taskl-img.png" mode=""></image></view>
				</view>
				<view class="imgList">
					<view class="lis"><image src="../../static/images/probleml-icon.png" mode=""></image></view>
				</view>
			</view>
			
			<view class="f5Bj-H20"></view>
			<view class="p-3 bg-white font-24">
				<view class="d-flex a-center">
					<image class="gpsIcon" src="../../static/images/taskl-icon.png" mode=""></image>
					<view class="ml-1">20楼楼梯间</view>
				</view>
				<view class="d-flex a-center flex-wrap mt-2">
					<view>处理人：张丹</view>
					<view class="ml-5">负责人：李工</view>
				</view>
			</view>
			
			<view class="f5Bj-H20"></view>
			
			<view class="submitbtn" style="margin-top: 100rpx;">
				<button class="btn" type="default">提交</button>
			</view>
			
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
				is_show:false,
				imgData:3
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/order.css";
	
	.imgList .lis{width: 210rpx;height: 210rpx; position: relative;}
	.closebtn{z-index: 22;background-color: #000;border-radius: 50%;width: 36rpx;height: 36rpx;color: #fff;display: flex;justify-content: center; align-items: center;opacity: 0.5;font-size: 36rpx;top: -14rpx;right: -14rpx;}
</style>
