<template>
	<view>
		<view class="top">
			<view class="cont">
				<img class="userPic" src="../../static/images/about-img.png" alt="">
				<view class="name">用户名称</view>
			</view>
		</view>
		
		<view class="myOrder">
			<view class="bigtit"  @click="webSelf.$Router.navigateTo({route:{path:'/pages/myOderList/myOderList'}})">我的订单 <text class="more">查看&gt;</text></view>
			<view class="orderNav">
				<view class="item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myOderList/myOderList'}})">
					<image src="../../static/images/about-icon1.png" alt=""/>
					<view>待支付</view>
				</view>
				<view class="item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myOderList/myOderList'}})">
					<image src="../../static/images/about-icon2.png" alt=""/>
					<view>待发货</view>
				</view>
				<view class="item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myOderList/myOderList'}})">
					<image src="../../static/images/about-icon3.png" alt=""/>
					<view>待收货</view>
				</view>
				<view class="item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myOderList/myOderList'}})">
					<image src="../../static/images/about-icon4.png" alt=""/>
					<view>已完成</view>
				</view>
			</view>
		</view>
		
		<view class="itemLisline" @click="webSelf.$Router.navigateTo({route:{path:'/pages/myAddress/myAddress'}})">
			<image class="icon" src="../../static/images/about-icon5.png" mode=""></image>
			<view class="tex">收货地址</view>
			<view class="arrow">
				<image src="../../static/images/arrow.png" mode=""></image>
			</view>
		</view>
		<view class="itemLisline" @click="webSelf.$Router.navigateTo({route:{path:'/pages/customerService/customerService'}})">
			<image class="icon" src="../../static/images/about-icon6.png" mode=""></image>
			<view class="tex">联系客服</view>
			<view class="arrow">
				<image src="../../static/images/arrow.png" mode=""></image>
			</view>
		</view>
	
	<!--底部tab键-->
	<view class="navbar">
		<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/index/index'}})">
			<view class="nav_img">
				<image src="../../static/images/nabar.png" />
			</view>
			<view class="text">首页</view>
		</view>
		<view class="navbar_item">
			<view class="nav_img" @click="webSelf.$Router.redirectTo({route:{path:'/pages/marketProdlist/marketProdlist'}})">
				<image src="../../static/images/nabar2.png" />
			</view>
			<view class="text">商品</view>
		</view>
		<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/car/car'}})">
			<view class="nav_img">
				<image src="../../static/images/nabar3.png" />
			</view>
			<view class="text">购物车</view>
		</view>
		<view class="navbar_item"   @click="webSelf.$Router.redirectTo({route:{path:'/pages/myCenter/myCenter'}})">
			<view class="nav_img">
				<image src="../../static/images/nabar4-a.png" />
			</view>
			<view class="text this-text">我的</view>
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
.top{width: 100%;height: 277rpx; background: #c6112b; display: flex; padding-bottom: 30rpx;}
.userPic{ width: 120rpx; height: 120rpx;border-radius: 50%;}
.top .cont{ display: flex;justify-content: flex-start; align-items: center;padding: 0 3%;}
.top .name{ display: inline-block; line-height: 120rpx; font-size: 28rpx;color: #fff; margin-left: 20rpx;width: 300rpx;}
.myOrder{width: 94%;margin: 0 auto;background: #fff;border-radius: 20rpx; box-shadow: 0 0 10rpx rgba(0,0,0,0.1); margin-top: -78rpx;padding: 20rpx;box-sizing: border-box; margin-bottom: 20rpx;}
.bigtit{ font-size: 30rpx;color: #4a4a4a; height: 60rpx; line-height: 60rpx;border-bottom: 2rpx solid #e7e7e7;}
.bigtit .more{ font-size: 24rpx; color: #999; float: right;}
.orderNav{display: flex;flex-wrap: nowrap; justify-content:space-between;padding: 20rpx 0;}
.orderNav .item{ text-align: center; width: 25%; font-size: 24rpx; color: #666;}
.orderNav .item image{ width: 54rpx; height: 54rpx;margin: 10rpx auto;}	

.itemLisline{height: 100rpx; display: flex;margin: 0 4%; align-items: center;border-bottom: 2rpx solid #e7e7e7; }
.itemLisline:first-child{border-top:10rpx solid red;}
.itemLisline .icon{ width: 30rpx; height: 30rpx; display: inline-block; margin-right: 20rpx;}
.itemLisline .tex{ width: 70%;font-size: 28rpx;}
.itemLisline .arrow{ width: 20%; align-items:flex-end; text-align: right;}
.itemLisline .arrow image{width: 15rpx; height: 30rpx; }
</style>
