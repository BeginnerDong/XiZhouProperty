<template>
	<view>
		
		<view class="">
			<view class="item bg-white px-3 py-3 rounded10 font-24">
				<view class="font-26">凤凰健康的首付款如何法规的数据库官方回复空间大使馆副高房价的合格刚恢复健康的航空凤凰健康的首付款如何法规的数据库官方回复空间大使馆副高房价的合格刚恢复健康的航空GV发</view>
				<view class="imgList d-flex a-start flex-wrap">
					<view class="lis" v-for="(item,index) in imgData" :key="index"><image src="../../static/images/taskl-img.png" mode=""></image></view>
				</view>
				<view class="d-flex a-center j-sb mt-3">
					<view class="color6">2020.05.18 16:00</view>
					<view class="red">未处理</view>
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
			<view class="p-3 bg-white">
				<view class="font-30 font-weight">处理结果</view>
				<view class="font-26 py-2">发货的数据库恢复入库单是否认可姐就说规范化进度款挂号费刚好放假肯定会刚好放假肯定会个更好发动机看后感</view>
				<view class="imgList d-flex a-start flex-wrap">
					<view class="lis"><image src="../../static/images/taskl-img.png" mode=""></image></view>
					<view class="lis"><image src="../../static/images/taskl-img.png" mode=""></image></view>
				</view>
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
	/* page{background: #F5F5F5;} */
	
	.imgList .lis{width: 210rpx;height: 210rpx;}
</style>
