<template>
	<view>
		<view>
			<view class="meallLis" v-for="(item,index) in meallLis" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlistDetail/ticketingProdlistDetail'}})">
				<view class="twoCt">
					<view class="leftbox">
						<image :src="item.imgUrl"></image>
					</view>
					<view class="cont">
						<view class="title avoidOverflow2">{{item.title}}</view>
						<view class="price">{{item.price}}</view>
					</view>
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketing/ticketing'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar1.png" />
				</view>
				<view class="text">门票预订</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingMeals/ticketingMeals'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar2-a.png" />
				</view>
				<view class="text  this-text">餐食预订</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingCenter/ticketingCenter'}})">
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
				score: '',
				wx_info: {},
				meallLis:[
					{
						imgUrl:"../../static/images/yuyue-img1.png",
						title:"标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题",
						price:"56.00"
					},
					{
						imgUrl:"../../static/images/yuyue-img1.png",
						title:"2222标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题",
						price:"88.00"
					},
					{
						imgUrl:"../../static/images/yuyue-img1.png",
						title:"3222标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题",
						price:"108.00"
					}
				]
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '餐食预约'
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
	@import "../../assets/style/car.css";

	page {
		padding-bottom: 140rpx;
		background: #f5f5f5;
	}
	.navbar .navbar_item{ width: 33.3%;}
	
	.meallLis{ margin:30rpx 4% 0 4%; background: #fff; border-radius: 10rpx;padding: 30rpx;}
	.meallLis .twoCt{ width: 100%;}
	.meallLis .twoCt .leftbox{ width: 180rpx; height: 180rpx;}
	.meallLis .twoCt .cont{ width: 420rpx; margin-left: 20rpx;}
</style>
