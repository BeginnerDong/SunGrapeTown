<template>
	<view class="" style="padding-bottom: 140rpx;">
		<view>
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration"
			 indicator-active-color="#db0160">
				<block v-for="(item,index) in imgUrls" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>

		<view class="detailTit">
			<view class="tit">标题标题标题标题标题标题标题标题</view>
			<view class="pric">56.00</view>
		</view>
		<view class="detailTit">
			<view class="flexRowBetween">
				<view class="spxq " style="margin-bottom: 0;">选择日期</view>
				<view class="Rmore" @click="showSel">查看更多&gt;</view>
			</view>
			<view class="seltDatabox flexRowBetween">
				<view class="mm">08-20</view>
				<view class="mm">08-21</view>
				<view class="mm">08-22</view>
				<view class="mm">...</view>
			</view>
		</view>
		<view class="xqInfor">
			<view class="spxq">商品详情</view>
			<view class="cont">
				<view>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
				<view>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
				<view>
					<image src="../../static/images/details-img2.png" mode="widthFix" />
				</view>
			</view>

		</view>

		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar" >
			<view class="xdyyBtn" @click="showSel">下单预约</view>
		</view>
		
		<view class="showSel" v-if="is_show" >
			<view class="mainbox fix">
				<view class="colseBtn" @click="showSel">×</view>
				<view class="twoCt">
					<view class="leftbox">
						<image src="../../static/images/yuyue-img1.png"></image>
					</view>
					<view class="cont">
						<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
						<view class="price">59.00</view>
					</view>
				</view>
				<view class="yy-title" style="padding: 20px 0 10px 0;">日期</view>
				<view class="yyDate-lis flexRowBetween" style="flex-wrap: wrap; justify-content: flex-start;">
					<view>08-20</view>
					<view>08-20</view>
					<view>08-20</view>
					<view>08-20</view>
					<view>08-20</view>
					<view>08-20</view>
					<view>08-20</view>
				</view>
				<view class="flexRowBetween" style="padding: 30rpx 0; border-top: 2rpx solid #e7e7e7; border-bottom: 2rpx solid #e7e7e7; margin-top: 10rpx;">
					<view class="yy-title">购买数量</view>
					<view class="numBox" style="position: relative; right: auto; bottom: auto;">
						<view>+</view>
						<view class="num">{{proNum}}</view>
						<view>-</view>
					</view>
				</view>
				<view class="submitbtn">
					<button type="submit" @click="webSelf.$Router.navigateTo({route:{path:'/pages/placeOrder/placeOrder'}})" >下单预约</button>
				</view>
			</view>
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				is_show: false,
				score: '',
				proNum:0,
				wx_info: {},
				imgUrls: [
					'../../static/images/details-img1.png',
					'../../static/images/details-img2.png',
					'../../static/images/home-banner.png'
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
			},
			showSel(){
				const self = this;
				self.is_show = !self.is_show;
				self.seltData({
					is_show:self.is_show
				})
			}
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/index.css";
	@import "../../assets/style/car.css";
	@import "../../assets/style/shopDetail.css";
	

</style>
