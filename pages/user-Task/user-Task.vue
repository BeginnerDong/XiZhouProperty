<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="orderNav bg-white shadow-sm color6 mb-3 d-flex j-sb a-center">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部({{total}})</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">未处理({{noDeal}})</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已处理({{hasDeal}})</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view class="seachbox d-flex a-center j-sb px-3 pt-3" v-if="userInfoData.behavior==2">
			<view class="d-flex a-center rr bg-white" style="width: 100%;">
				<button class="seachBtn" type="button"><image src="../../static/images/fenleilingdao-icon.png" mode=""></image></button>
				<view class="input">
					<input type="text" v-model="name" @confirm="search" placeholder="搜索姓名" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		
		<view class="d-flex a-center flex-column mx-3 bg-white rounded10 mt-3 px-3 py-2" style="height: 118rpx;" v-if="userInfoData.behavior==1">
			<view class="text-center d-flex a-center font-24 color9">时间筛选<image class="arrowB ml" src="../../static/images/yuangong-icon.png" mode=""></image></view>
			<view class="value mt-1 font-26"><DatetimePicker  
		@change="changeDatetimePicker" placeholder="选择日期时间" fields="day"></DatetimePicker></view>
		</view>
		
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
					<view class="underBtn d-flex j-end a-center py-3"  v-if="item.deal==0&&userInfoData.behavior==1">
						<view class="Bbtn" 
						:data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/user-staffDealing/user-staffDealing?id='+$event.currentTarget.dataset.id}})">
						确认处理
						</view>
					</view>
				</view>
		
			</view>
		</view>
		
		
		<!-- 无数据 -->
		<view class="nodata"  v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
	
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
				Router: this.$Router,
				curr: 1,
				mainData:[],
				total:0,
				hasDeal:0,
				noDeal:0,
				searchItem:{
					thirdapp_id:2,
					//user_type:0
					behavior:1
				},
				userInfoData:{},
				name:'',
				getBefore:{},
				timeStamp:0
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		onPullDownRefresh() {
			console.log('refresh');
			const self = this;
			if(self.userInfoData.behavior==1){
				self.timeStamp = 0
			}else if(self.userInfoData.behavior==2){
				self.getBefore = {}
			};
			self.getMainData(true)
		},
		
		methods: {
			
			search(){
				const self = this;
				if(self.name!=''){
					self.getBefore = {
						name:{
							tableName:'UserInfo',
							middleKey:'deal_no',
							key:'user_no',
							searchItem:{
								name:['in',[self.name]]
							},
							condition:'in'
						}
					};
					self.getMainData(true)
				}
			},
			
			changeDatetimePicker(e){
				console.log(e)
				const self = this;
				self.timeStamp = self.$Utils.timeToTimestamp(e.fmt3);
				console.log(self.timeStamp);
				self.getMainData(true)
			},
			
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
						if(self.userInfoData.behavior==1){
							self.searchItem.deal_no = wx.getStorageSync('user_info').user_no
						}else if(self.userInfoData.behavior==2){
							self.searchItem.charge_no = wx.getStorageSync('user_info').user_no
						}
						self.getMainData(true);
					}
					
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
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
				if(JSON.stringify(self.getBefore)!='{}'){
					postData.getBefore = self.$Utils.cloneForm(self.getBefore);
				};
				if(self.timeStamp>0){
					postData.searchItem.create_time = ['between',[self.timeStamp,self.timeStamp+86400]]
				};
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
							behavior:1
						}
					],
					hasDeal: [
						'count',
						'count',
						{
							thirdapp_id: 2,
							deal: 1,
							behavior:1
						}
					],
					total: [
						'count',
						'count',
						{
							thirdapp_id: 2,
							//deal: 1
							behavior:1
						}
					],
				};
				if(self.userInfoData.behavior==1){
					postData.compute.noDeal[2].deal_no = wx.getStorageSync('user_info').user_no
					postData.compute.hasDeal[2].deal_no = wx.getStorageSync('user_info').user_no
					postData.compute.total[2].deal_no = wx.getStorageSync('user_info').user_no
				}else if(self.userInfoData.behavior==2){
					postData.compute.noDeal[2].charge_no = wx.getStorageSync('user_info').user_no
					postData.compute.hasDeal[2].charge_no = wx.getStorageSync('user_info').user_no
					postData.compute.total[2].charge_no = wx.getStorageSync('user_info').user_no
				}
				if(self.timeStamp>0){
					postData.compute.noDeal[2].create_time = ['between',[self.timeStamp,self.timeStamp+86400]]
					postData.compute.hasDeal[2].create_time = ['between',[self.timeStamp,self.timeStamp+86400]]
					postData.compute.total[2].create_time = ['between',[self.timeStamp,self.timeStamp+86400]]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);	
					};
					if(JSON.stringify(self.getBefore)=='{}'){
						self.total = res.info.compute.total;
						self.noDeal = res.info.compute.noDeal;
						self.hasDeal =  res.info.compute.hasDeal;
					}else{
						self.searchByStaff()
					}
					setTimeout(function() {
						uni.stopPullDownRefresh();
					}, 1000);
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			searchByStaff() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||wx.getStorageSync('user_info').headImgUrl==''||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				postData.searchItem = {
					name:self.name
				};
				const callback = (res) => {
					if(res.solely_code==100000){
						self.total = res.info.total;
						self.noDeal = res.info.undeal;
						self.hasDeal =  res.info.deal;
					}
				};
				self.$apis.searchByStaff(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/order.css";
	@import "../../assets/style/seach.css";
	page{background: #F5F5F5;}
	
	.underBtn .Bbtn{display: block;width: 160rpx;line-height: 50rpx; border:1px solid #d2d2d2; text-align: center; margin-left: 30rpx; border-radius: 10rpx; font-size: 26rpx;box-sizing: border-box; color: #666;}
	.underBtn .Bbtn.blue{color: #38a0f9;border-color: #38a0f9;}
</style>
