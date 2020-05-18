<template>
	<view>

		<view class="xqInfor mx-3">
			<view class="font-36 font-weight text-center pt-4 pb-3">长青白</view>
			<view class="cont">
				<view>速度快和个人防护施工技术规划局发货的顾客发动机盖日工行卡拿过来是符合大家可舒服了回房间的思考和繁荣街事儿涸鲋得水尽快发货克里斯多夫GV回来是恢复健康的说法幅度飞机客人都湿了刚和艰苦奋斗，个刚恢复健康大哥恢复健康的两个刚和艰苦奋斗很给力股，积分的快感回复附近的开始。</view>
				<view class="">
					<image class="w" src="../../static/images/home-img.png" mode="widthFix"></image>
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
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/detail.css";
</style>
