<template>
	<view>

		<view class="topNavFix f5bj">
			<view class="orderNav bg-white shadow color6 mb-3 d-flex j-sb a-center">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部({{total}})</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">未处理({{noDeal}})</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已处理({{hasDeal}})</view>
			</view>
		</view>
		<view class="topNavH"></view>

		<view class="mx-3">
			<view class="orderList">
				<view class="item bg-white mt-3 px-3 py-3 rounded10 font-24" v-for="(item,index) in mainData" :key="index">
					<view :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/user-orderDetail/user-orderDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="avoidOverflow2 font-26">{{item.description}}</view>
						<view class="imgList d-flex a-start flex-wrap">
							<view class="lis" v-for="(c_item,c_index) in item.mainImg">
								<image :src="c_item.url" mode=""></image>
							</view>
							
						</view>
						<view class="d-flex a-center j-sb mt-3">
							<view class="color6 a-center d-flex">{{item.create_time}}<span class="ml-2">{{item.typeName.title}}</span></view>
							<view class="red" v-show="item.deal==0">未处理</view>
							<view class="red" v-show="item.deal==1">已处理</view>
						</view>
						<view class="mt-3 dasheTop pt-3">
							<view class="d-flex a-center">
								<image class="gpsIcon" src="../../static/images/taskl-icon.png" mode=""></image>
								<view class="ml-1">{{item.placeName.name?item.placeName.name:'无'}}</view>
							</view>
							<view class="d-flex a-center flex-wrap mt-2">
								<view>处理人：{{item.dealName.name?item.dealName.name:'无'}}</view>
								<view class="ml-5">负责人：{{item.chargeName.name?item.chargeName.name:'无'}}</view>
							</view>
						</view>
					</view>
				</view>

			</view>
		</view>


		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				curr: 1,
				mainData:[],
				total:0,
				hasDeal:0,
				noDeal:0,
				searchItem:{
					thirdapp_id:2
				}
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},

		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},



		methods: {
			changeCurr(curr) {
				const self = this;
				if (curr != self.curr) {
					self.curr = curr;
					if(self.curr==1){
						delete self.searchItem.deal
					}else if(self.curr==2){
						self.searchItem.deal = 0
					}else if(self.curr==3){
						self.searchItem.deal = 1
					};
					self.getMainData(true)
				}
			},

			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getAfter = {
					dealName:{
						tableName:'UserInfo',
						middleKey:'deal_no',
						key:'user_no',
						condition:'=',
						searchItem:{
							status:1
						},
						info:['name']
					},
					chargeName:{
						tableName:'UserInfo',
						middleKey:'charge_no',
						key:'user_no',
						condition:'=',
						searchItem:{
							status:1
						},
						info:['name']
					},
					placeName:{
						tableName:'Place',
						middleKey:'relation_id',
						key:'id',
						condition:'=',
						searchItem:{
							status:1
						},
						info:['name']
					},
					typeName:{
						tableName:'Label',
						middleKey:'type',
						key:'id',
						condition:'=',
						searchItem:{
							status:1
						},
						info:['title']
					}
				}
				postData.compute = {
					noDeal: [
						'count',
						'count',
						{
							thirdapp_id: 2,
							deal: 0,
							user_no:uni.getStorageSync('user_info').user_no
						}
					],
					hasDeal: [
						'count',
						'count',
						{
							thirdapp_id: 2,
							deal: 1,
							user_no:uni.getStorageSync('user_info').user_no
						}
					],
					total: [
						'count',
						'count',
						{
							thirdapp_id: 2,
							//deal: 1,
							user_no:uni.getStorageSync('user_info').user_no
						}
					],
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						
					}
					self.total = res.info.compute.total;
					self.noDeal = res.info.compute.noDeal;
					self.hasDeal =  res.info.compute.hasDeal;
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

	page {
		background: #F5F5F5;
	}
</style>
