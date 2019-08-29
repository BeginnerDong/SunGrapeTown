<template>
	<view>
		<view class="tick-top pr">
			<image class="bj" src="../../static/images/1about-img1.png"></image>
			<view class="info">
				<img class="photo" src="../../static/images/about-img.png" alt="">
				<view>太阳葡萄</view>
			</view>
		</view>
		
		<view class="tick-nav flexRowBetween">
			<view class="child">
				<image src="../../static/images/1about-icon1.png" alt="" 
				@click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingMyBook/ticketingMyBook?id=3'}})"/>
				<view>我的预订</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingMyBook/ticketingMyBook?id=2'}})">
				<image src="../../static/images/1about-icon2.png" alt=""/>
				<view>我的门票</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/customerService/customerService'}})">
				<image src="../../static/images/1about-icon3.png" alt=""/>
				<view>联系客服</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/ticketing/ticketing'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar1.png" />
				</view>
				<view class="text">门票预订</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/ticketingMeals/ticketingMeals'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar2.png" />
				</view>
				<view class="text">餐食预订</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/ticketingCenter/ticketingCenter'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar3-a.png" />
				</view>
				<view class="text  this-text">我的</view>
			</view>
		</view>
		<!--底部tab键-->

	</view>

</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false,
				score: '',
				wx_info: {}
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '我的'
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
.navbar .navbar_item{ width: 33.3%;}
page{background: #f5f5f5;}
.tick-top{ background: #fff;}
.tick-top .bj{ width: 100%; height: 293rpx; display: block; position: absolute; top: 0;left: 0; right: 0; z-index: 0;}
.tick-top .info{ padding-top:200rpx ; position: relative; z-index: 2; text-align: center; font-size: 30rpx;}
.tick-top .info .photo{ width:124rpx ; height: 124rpx; border-radius: 50%; margin-bottom: 10rpx;}
.tick-nav{ margin: 50rpx 4%; background: #fff;padding:50rpx 40rpx;box-sizing: border-box;border-radius: 10rpx; box-shadow: 0 0 8rpx rgba(0,0,0,0.1);}
.tick-nav .child{ width: 33.3%; text-align: center; font-size: 26rpx;}
.tick-nav .child image{ width: 75rpx;  height: 75rpx; margin: 0 auto 15rpx auto;}

</style>
