<template>
	<view>
		
		<view class="">
			<view class="item bg-white px-3 py-3 rounded10 font-24">
				<view class="font-26">{{mainData.description}}</view>
				<view class="imgList d-flex a-start flex-wrap">
					<view class="lis" v-for="(item,index) in mainData.mainImg" :key="index">
						<image :src="item.url" mode=""></image></view>
				</view>
				<view class="d-flex a-center j-sb mt-3">
					<view class="color6">{{mainData.create_time}}</view>
					<view class="red" v-if="mainData.behavior==1">{{mainData.deal==0?'未处理':'已处理'}}</view>
					<view class="red" v-if="mainData.behavior==2">巡检正常</view>
				</view>
			</view>
			
			<view class="f5Bj-H20"></view>
			<view class="p-3 bg-white font-24">
				<view class="d-flex a-center">
					<image class="gpsIcon" src="../../static/images/taskl-icon.png" mode=""></image>
					<view class="ml-1">{{mainData.placeName.name?mainData.placeName.name:'无'}}</view>
				</view>
				<view class="d-flex a-center flex-wrap mt-2">
					<view>处理人：{{mainData.dealName.name?mainData.dealName.name:'无'}}</view>
					<view class="ml-5">负责人：{{mainData.chargeName.name?mainData.chargeName.name:'无'}}</view>
				</view>
			</view>
			
			<view class="f5Bj-H20"></view>
			<view class="p-3 bg-white" v-if="mainData.deal==1">
				<view class="font-30 font-weight">处理结果</view>
				<view class="font-26 py-2">{{mainData.content}}</view>
				<view class="imgList d-flex a-start flex-wrap">
					<view class="lis" v-for="(item,index) in mainData.bannerImg" :key="index">
						<image :src="item.url" mode=""></image></view>
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
				mainData:{},
				
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				postData.getAfter = {
					dealName: {
						tableName: 'UserInfo',
						middleKey: 'deal_no',
						key: 'user_no',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['name']
					},
					chargeName: {
						tableName: 'UserInfo',
						middleKey: 'charge_no',
						key: 'user_no',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['name']
					},
					createName: {
						tableName: 'UserInfo',
						middleKey: 'user_no',
						key: 'user_no',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['name']
					},
					placeName: {
						tableName: 'Place',
						middleKey: 'relation_id',
						key: 'id',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['name']
					},
					typeName: {
						tableName: 'Label',
						middleKey: 'type',
						key: 'id',
						condition: '=',
						searchItem: {
							status: 1
						},
						info: ['title']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						//self.submitData.mainImg = self.mainData.mainImg;
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/order.css";
	/* page{background: #F5F5F5;} */
	
	.imgList .lis{width: 210rpx;height: 210rpx;}
</style>
