<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="orderNav bg-white shadow color6 mb-3 d-flex j-sb a-center">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部(2)</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">正常(1)</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">问题(1)</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view class="mx-3">
			<view class="orderList">
				<view class="item bg-white mt-3 px-3 py-3 rounded10 font-24" v-for="(item,index) in inspectData" :key="index">
					<view @click="Router.navigateTo({route:{path:'/pages/user-orderDetail/user-orderDetail'}})">
						<view class="avoidOverflow2 font-26">凤凰健康的首付款如何法规的数据库官方回复空间大使馆副高房价的合格刚恢复健康的航空GV发价的合格刚恢复健康的航空</view>
						<view class="imgList d-flex a-start flex-wrap">
							<view class="lis"><image src="../../static/images/taskl-img.png" mode=""></image></view>
							<view class="lis"><image src="../../static/images/taskl-img.png" mode=""></image></view>
							<view class="lis"><image src="../../static/images/taskl-img.png" mode=""></image></view>
						</view>
						<view class="d-flex a-center j-sb mt-3">
							<view class="color6 a-center d-flex">2020.05.18 <span class="ml-2">保洁类</span></view>
							<view class="red" v-show="curr==1||curr==2">正常</view>
							<view class="red" v-show="curr==3">问题</view>
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
					
					<view class="dasheTop d-flex a-center j-end color9 mt-3 pt-3">
						<view class="d-flex a-center" @click="Router.navigateTo({route:{path:'/pages/user-orderDetailEdit/user-orderDetailEdit'}})"><image class="editIcon mr" src="../../static/images/fenleilingdao-icon-02.png" mode=""></image>编辑</view>
						<view class="d-flex a-center ml-5" @click="deleteAll()"><image class="editIcon mr" src="../../static/images/fenleilingdao-icon-01.png" mode=""></image>删除</view>
					</view>
				</view>
				
			</view>
		</view>
		
		<!-- 分类领导  巡检中的添加问题按钮 -->
		<view class="R-fixIcon" @click="Router.navigateTo({route:{path:'/pages/user-inspectAdd/user-inspectAdd'}})"><image src="../../static/images/fenleilingdao-icon1.png" mode=""></image></view>
		
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
	
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
				curr:1,
				inspectData:1
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
			},
			deleteAll() {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认要删除选中商品吗？',
					showCancel: true,
					cancelText: '取消',
					confirmText: '确认',
					success: res => {
						if (res.confirm) {
							for (var i = 0; i < self.mainData.length; i++) {
								if(self.mainData[i].isSelect){
									self.$Utils.delStorageArray('inspectData', self.mainData[i], 'id');
								}
							};
							self.mainData = self.$Utils.getStorageArray('inspectData');
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					},
				});
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/order.css";
	page{background: #F5F5F5;}
	
	
</style>
