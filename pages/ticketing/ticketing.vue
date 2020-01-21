<template>
	<view>
		<view class="topWhite">
			<view style="border-bottom: 20rpx solid #f5f5f5;">
				<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration"
				 indicator-active-color="#db0160">
					<block v-for="(item,index) in mainData" :key="index">
						<swiper-item  class="swiper-item" >
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image"/>
						</swiper-item>
					</block> 
				</swiper>
			</view>
		</view>
		<view class="homeIcon flexRowBetween" style="flex-wrap: wrap;">
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist?title=亲子双人票'}})">
				<image src="../../static/images/1home-icon1.png" alt=""/>
				<view>亲子双人票</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist?title=亲子三人票'}})">
				<image src="../../static/images/1home-icon2.png" alt=""/>
				<view>亲子三人票</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist?title=林地露营'}})">
				<image src="../../static/images/1home-icon3.png" alt=""/>
				<view>林地露营</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist?title=真人CS'}})">
				<image src="../../static/images/1home-icon4.png" alt=""/>
				<view>真人CS</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist?title=丛林穿越'}})">
				<image src="../../static/images/1home-icon5.png" alt=""/>
				<view>丛林穿越</view>
			</view>
			<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlist/ticketingProdlist?title=沙滩摩托'}})">
				<image src="../../static/images/1home-icon6.png" alt=""/>
				<view>沙滩摩托</view>
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
				
				mainData:[],
				indicatorDots: false,
				autoplay: false,
				interval: 5000,
				duration: 1000
			}
		},

		onLoad(options) {
			const self = this;
		
			self.$Utils.loadAll(['getMainData'], self);
		},

		onShow() {
			const self = this;
			document.title = '门票预订'
		},

		methods: {
		

			getMainData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
					},
				};
				postData.getBefore = {
					parent: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['门票预订轮播']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
					}
					console.log('res', res)
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.labelGet(postData, callback);
			},
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
