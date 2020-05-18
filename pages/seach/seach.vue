<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox d-flex a-center j-sb f5bj px-3 py-3"> 
			<view class="d-flex a-center rr f5bj bg-white" style="width: 85%;">
				<button class="seachBtn" type="button"><image src="../../static/images/fenleilingdao-icon.png" mode=""></image></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="搜索姓名" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="font-30 main-text-color">搜索</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false
			}
		},
		
		onLoad(options) {
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
	@import "../../assets/style/seach.css";
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}
	
</style>

