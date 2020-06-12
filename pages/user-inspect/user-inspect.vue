<template>
	<view>

		<view class="topNavFix f5bj">
			<view class="orderNav bg-white shadow color6 mb-3 d-flex j-sb a-center">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部({{total}})</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">正常({{normal}})</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">问题({{question}})</view>
			</view>
		</view>
		<view class="topNavH"></view>

		<view class="mx-3">
			<view class="orderList">
				<view class="item bg-white mt-3 px-3 py-3 rounded10 font-24" v-for="(item,index) in mainData" :key="index">
					<view :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/user-orderDetail/user-orderDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="avoidOverflow2 font-26">{{item.description}}</view>
						<view class="imgList d-flex a-start flex-wrap">
							<view class="lis" v-for="(c_item,c_index) in item.mainImg">
								<image :src="c_item.url" mode=""></image>
							</view>
						</view>
						<view class="d-flex a-center j-sb mt-3">
							<view class="color6 a-center d-flex">{{item.create_time}}<span class="ml-2">{{item.typeName.title}}</span></view>
							<view class="red" v-show="item.behavior==2">正常</view>
							<view class="red" v-show="item.behavior==1">问题</view>
						</view>
						<view class="mt-3 dasheTop pt-3">
							<view class="d-flex a-center">
								<image class="gpsIcon" src="../../static/images/taskl-icon.png" mode=""></image>
								<view class="ml-1">{{item.placeName.name?item.placeName.name:'无'}}</view>
							</view>
							<view class="d-flex a-center flex-wrap mt-2">
								<view>处理人：{{item.dealName.name?item.dealName.name:'无'}}</view>
								<view class="ml-5">负责人：{{item.chargeName.name?item.chargeName.name:'无'}}</view>
								<view class="ml-5">提交人：{{item.createName.name?item.createName.name:'无'}}</view>
							</view>
						</view>
					</view>

					<view class="dasheTop d-flex a-center j-end color9 mt-3 pt-3" v-if="type==2">
						<view class="d-flex a-center" :data-id="item.id"
						@click="Router.navigateTo({route:{path:'/pages/user-orderDetailEdit/user-orderDetailEdit?id='+$event.currentTarget.dataset.id}})">
							<image class="editIcon mr" src="../../static/images/fenleilingdao-icon-02.png" mode=""></image>编辑
						</view>
						<view class="d-flex a-center ml-5" @click="deleteThis(index)">
							<image class="editIcon mr" src="../../static/images/fenleilingdao-icon-01.png" mode=""></image>删除
						</view>
					</view>
				</view>

			</view>
		</view>

		<!-- 分类领导  巡检中的添加问题按钮 -->
		<view class="R-fixIcon" v-if="type==2" @click="Router.navigateTo({route:{path:'/pages/user-inspectAdd/user-inspectAdd'}})">
			<image src="../../static/images/fenleilingdao-icon1.png" mode=""></image>
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
				mainData: [],
				total: 0,
				question: 0,
				normal: 0,
				searchItem: {
					thirdapp_id: 2
				},
				type: ''
			}
		},

		onLoad(options) {
			const self = this;
			self.type = options.type;
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
					if (self.curr == 1) {
						delete self.searchItem.behavior
					} else if (self.curr == 2) {
						self.searchItem.behavior = 2
					} else if (self.curr == 3) {
						self.searchItem.behavior = 1
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
				if (self.type && self.type == 2) {
					postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				} else if (self.type && self.type == 3) {
					postData.searchItem.class = 3;
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
				}
				postData.compute = {
					normal: [
						'count',
						'count',
						{
							thirdapp_id: 2,
							behavior: 2,
							//user_no
							status:1
						}
					],
					question: [
						'count',
						'count',
						{
							thirdapp_id: 2,
							behavior: 1,
							status:1
						}
					],
					total: [
						'count',
						'count',
						{
							thirdapp_id: 2,
							//behavior: 1
							status:1
						}
					],
				};
				if (self.type && self.type == 2) {
					postData.compute.normal[2].user_no = uni.getStorageSync('user_info').user_no
					postData.compute.question[2].user_no = uni.getStorageSync('user_info').user_no
					postData.compute.total[2].user_no = uni.getStorageSync('user_info').user_no
					//postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				} else if (self.type && self.type == 3) {
					postData.compute.normal[2].class = 3
					postData.compute.question[2].class = 3 
					postData.compute.total[2].class = 3
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);

					}
					self.total = res.info.compute.total;
					self.normal = res.info.compute.normal;
					self.question = res.info.compute.question;
					self.$Utils.finishFunc('getMainData');

				};
				self.$apis.messageGet(postData, callback);
			},

			deleteThis(index) {
				const self = this;
				const postData = {};
				uni.showModal({
					title: '提示',
					content: '确认要删除此条数据吗？',
					showCancel: true,
					cancelText: '取消',
					confirmText: '确认',
					success: res => {
						if (res.confirm) {
							postData.tokenFuncName = 'getProjectToken';
							postData.searchItem = {
								id: self.mainData[index].id
							};
							postData.data = {
								status: -1
							};
							const callback = (res) => {
								if(res.solely_code==100000){
									self.$Utils.showToast('操作成功', 'none', 1000)
									setTimeout(function() {
										self.getMainData(true)
									}, 1000);
								}
							};
							self.$apis.messageUpdate(postData, callback);
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
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
