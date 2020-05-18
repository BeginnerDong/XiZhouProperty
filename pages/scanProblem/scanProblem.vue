<template>
	<view>

		<view class="editLine mx-3">
			<view class="item d-flex a-center j-sb" @click="Router.navigateTo({route:{path:'/pages//'}})" >
				<view class="ll">选择类型</view>
				<view class="rr d-flex a-center j-end" >
					<view class="d-flex a-center" @click="typeShow">
						<view class=" color9">请选择</view>
						<image class="arrowR" src="../../static/images/icon.png" mode=""></image>
					</view>
					
				</view>
			</view>
			<view class="item">
				<view>
					<textarea value="" placeholder="详情描述" placeholder-class="placeholder" />
				</view>
				
				<view class="picList d-flex a-start flex-wrap">
					<block v-for="(image,index) in imageList" :key="index">
						<view class="upImgLis">
							<image :src="image" :data-src="image" @tap="previewImage"></image>
						</view>
					</block>
					<view class="upBtn mt-2" @click="chooseImage"><image src="../../static/images/probleml-icon.png" mode=""></image></view>
				</view>
			</view>
			<view class="item  d-flex a-center j-sb">
				<view class="ll">是否匿名</view>
				<view class="rr d-flex a-center j-end">
					<view class="d-flex a-center mgr25" @click="setChange('1')">
						<image class="setIcon" :src="seltCurr==1?'../../static/images/probleml-icon3.png':'../../static/images/probleml-icon2.png'" mode=""></image>
						<view>是</view>
					</view>
					<view class="d-flex a-center ml-5" @click="setChange('2')">
						<image class="setIcon" :src="seltCurr==2?'../../static/images/probleml-icon3.png':'../../static/images/probleml-icon2.png'" mode=""></image>
						<view>否</view>
					</view>
				</view>
			</view>
			<view class="item  d-flex a-center j-sb">
				<view class="ll">联系电话</view>
				<view class="rr">
					<input type="number" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class=" typeShow rounded10 bg-white font-26 px-3" v-show="is_typeShow">
			<view class="item d-flex a-center j-sb" v-for="(item,index) in typeArray" :key="index" @click="typeChange(index)">
				<view>{{item}}</view>
				<view class="icon"><image :src="typeCurr==index?'../../static/images/probleml-icon1.png':''" mode=""></image></view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="default">提交</button>
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
				imageList: [],
				seltCurr:0,
				is_typeShow:false,
				typeCurr:0,
				typeArray:['保洁','维修','绿化']
			}
		},
		onUnload() {
			this.imageList = []
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			typeShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_typeShow = !self.is_typeShow
			},
			typeChange(index){
				const self = this;
				self.typeCurr = index,
				self.is_show = false
				self.is_typeShow = false
			},
			chooseImage: async function() {
				uni.chooseImage({
					success: (res) => {
						this.imageList = this.imageList.concat(res.tempFilePaths);
					}
				})
			},
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
			setChange(seltCurr){
				const self = this;
				if(seltCurr!=self.seltCurr){
					self.seltCurr = seltCurr
				}
			},
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
	@import "../../assets/style/editInfor.css";
	.upBtn{width: 120rpx;height: 120rpx;}
	.upImgLis{width: 150rpx;height: 150rpx; margin: 0rpx 30rpx 30rpx 0;overflow: hidden;}
	.upImgLis:nth-of-type(4n){margin-right: 0;}
	.setIcon{width: 32rpx;height: 32rpx;display: block;margin-right: 10rpx;}
	.rr input{text-align: right;}
	
	.typeShow{width: 80%;position: fixed; top: 50%;left: 50%;z-index: 50;transform: translate(-50%,-50%);}
	.typeShow .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.typeShow .item:last-child{border-bottom: 0;}
	.typeShow .item .icon{width: 28rpx;height: 22rpx;}
</style>
