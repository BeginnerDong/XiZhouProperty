<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="orderNav bg-white shadow-sm color6 mb-3 d-flex j-sb a-center">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部(2)</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">未处理(1)</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已处理(1)</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view class="seachbox d-flex a-center j-sb px-3 pt-3">
			<view class="d-flex a-center rr bg-white" style="width: 100%;" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<button class="seachBtn" type="button"><image src="../../static/images/fenleilingdao-icon.png" mode=""></image></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="搜索姓名" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		
		<view class="d-flex a-center flex-column mx-3 bg-white rounded10 mt-3 px-3 py-2" style="height: 118rpx;"  @change="changeDatetimePicker">
			<view class="text-center d-flex a-center font-24 color9">时间筛选<image class="arrowB ml" src="../../static/images/yuangong-icon.png" mode=""></image></view>
			<view class="value mt-1 font-26"><DatetimePicker placeholder="选择日期时间" fields="day"></DatetimePicker></view>
		</view>
		
		<view class="mx-3">
			<view class="orderList">
				<view class="item bg-white mt-3 px-3 py-3 rounded10 font-24">
					<view @click="Router.navigateTo({route:{path:'/pages/user-orderDetail/user-orderDetail'}})">
						<view class="avoidOverflow2 font-26">凤凰健康的首付款如何法规的数据库官方回复空间大使馆副高房价的合格刚恢复健康的航空GV发价的合格刚恢复健康的航空</view>
						<view class="imgList d-flex a-start flex-wrap">
							<view class="lis"><image src="../../static/images/taskl-img.png" mode=""></image></view>
							<view class="lis"><image src="../../static/images/taskl-img.png" mode=""></image></view>
							<view class="lis"><image src="../../static/images/taskl-img.png" mode=""></image></view>
						</view>
						<view class="d-flex a-center j-sb mt-3">
							<view class="color6 a-center d-flex">2020.05.18 <span class="ml-2">保洁类</span></view>
							<view class="red" v-show="curr==1||curr==2">未处理</view>
							<view class="red" v-show="curr==3">已处理</view>
						</view>
						<view class="mt-3 dasheTop pt-3">
							<view class="d-flex a-center">
								<image class="gpsIcon" src="../../static/images/taskl-icon.png" mode=""></image>
								<view class="ml-1">20楼楼梯间</view>
							</view>
							<view class="d-flex a-center flex-wrap mt-2">
								<view>处理人：张丹</view>
								<view class="ml-5">负责人：李工</view>
							</view>
						</view>
					</view>
				</view>
				
			</view>
		</view>
		
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
	
	</view>
</template>

<script>
	import DatetimePicker from '@/components/datetime-picker/datetime-picker.vue';
	export default {
		components: {
			DatetimePicker
		},
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/order.css";
	@import "../../assets/style/seach.css";
	page{background: #F5F5F5;}
	
	
</style>
