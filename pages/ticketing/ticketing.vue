<template>
	<view>
		<view class="topWhite">
			<view style="border-bottom: 20rpx solid #f5f5f5;">
				<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration"
				 indicator-active-color="#db0160">
					<block v-for="(item,index) in imgUrls" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item" class="slide-image" />
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>
		<view class="homeIcon flexRowBetween" style="flex-wrap: wrap;">
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist'}})">
				<image src="../../static/images/1home-icon1.png" alt=""/>
				<view>亲子双人票</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist'}})">
				<image src="../../static/images/1home-icon2.png" alt=""/>
				<view>亲子三人票</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist'}})">
				<image src="../../static/images/1home-icon3.png" alt=""/>
				<view>林地露营</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist'}})">
				<image src="../../static/images/1home-icon4.png" alt=""/>
				<view>真人CS</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist'}})">
				<image src="../../static/images/1home-icon5.png" alt=""/>
				<view>丛林穿越</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist'}})">
				<image src="../../static/images/1home-icon6.png" alt=""/>
				<view>葡萄酒DIY</view>
			</view>
		</view>



		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/ticketing/ticketing'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar1-a.png" />
				</view>
				<view class="text this-text">门票预订</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/ticketingMeals/ticketingMeals'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar2.png" />
				</view>
				<view class="text">餐食预订</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/ticketingCenter/ticketingCenter'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar3.png" />
				</view>
				<view class="text">我的</view>
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
				is_show: false,
				score: '',
				wx_info: {},
				imgUrls: [
					'../../static/images/1home-banner.png',
					'../../static/images/1home-banner.png',
					'../../static/images/1home-banner.png'
				],
				indicatorDots: false,
				autoplay: false,
				interval: 5000,
				duration: 1000
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
	@import "../../assets/style/index.css";
	.navbar .navbar_item{ width: 33.3%;}
	.homeIcon{ padding: 20rpx 4%;}
	.homeIcon .child{ width: 33.3%; text-align: center; font-size: 26rpx; margin: 20rpx 0;}
	.homeIcon .child image{ width: 100rpx; height: 101rpx; margin:10rpx auto;}
</style>
