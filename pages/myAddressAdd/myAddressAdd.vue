<template>
	<view class="content">
		<view class="linelis">
			<view class="left-tit">收货人</view>
			<view class="right-ct">
				<input type="text" placeholder="请输入真实姓名">
			</view>
		</view>	
		<view class="linelis">
			<view class="left-tit">手机号码</view>
			<view class="right-ct" >
				<input style="text-align: right;" type="text" placeholder="+86">
			</view>
		</view>	
		<view class="linelis">
			<view class="left-tit">所在地区</view>
			<view class="right-ct" >
				<input style="text-align: right;" type="text" placeholder="省市区">
			</view>
		</view>	
		<view class="linelis" style="min-height: 100rpx; height: auto;">
			<view class="left-tit">详细地址</view>
			<view class="right-ct" >
				<textarea class="textarea"  bindblur="bindTextAreaBlur"  placeholder="如街道/门牌号"></textarea>
			</view>
		</view>	
		<view class="linelis">
			<view class="left-tit" style="width: 50%;">设为默认地址</view>
			<view class="right-ct" style="width:50%;text-align: right" >
				<img class="mmIcon" src="../../static/images/address-icon02.png" alt="">
				<img v-if="isshow" class="mmIcon" src="../../static/images/address-icon03.png" alt="">
			</view>
		</view>	
		
		<view class="submitbtn">
			<button type="button">保存</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false,
				isshow:false,
				score: '',
				wx_info: {}
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '小镇市集'
		},

		methods: {
			change() {
				const self = this;
			},

			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	.content{ padding: 0 4%;}
	.linelis{  height: 100rpx; display: flex; justify-content: space-between; align-items: center; border-bottom: 2rpx solid #e7e7e7;}
	.linelis .left-tit{ width: 20%; font-size: 28rpx; color: #333; line-height: 60rpx;}
	.right-ct{ width: 80%;}
	.right-ct input{ width: 100%; height: 60rpx ; line-height: 60rpx; padding: 0 20rpx; box-sizing: border-box; font-size: 28rpx;}
	.right-ct .textarea{ width: 100%; height: 100rpx!important; padding:10rpx 20rpx;font-size: 28rpx; display: block; box-sizing: border-box;  min-height:60rpx;padding-top: 34rpx;}
	.mmIcon{ width: 120rpx;  height: 50rpx; display:inline-block;}
</style>
