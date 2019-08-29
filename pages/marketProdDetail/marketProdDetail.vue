<template>
	<view class="" style="padding-bottom: 140rpx;">
		<view>
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration"
			 indicator-active-color="#db0160">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="detailTit">
			<view class="tit">{{mainData.title}}</view>
			<view class="pric">{{mainData.price}}</view>
		</view>
		
		<view class="xqInfor">
			<view class="spxq">商品详情</view>
			<view class="cont">
				<view class="content ql-editor" v-html="mainData.content" >
				</view>
			</view>

		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar">
			<view class="left">
				<view class="ite" @click="webSelf.$Router.redirectTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/details-icon1.png" mode=""></image>
					<view>返回首页</view>
				</view>
				<view class="ite" @click="webSelf.$Router.navigateTo({route:{path:'/pages/customerService/customerService'}})">
					<image src="../../static/images/details-icon2.png" mode=""></image>
					<view>客服</view>
				</view>
				<view class="ite" @click="addCart()">
					<image src="../../static/images/details-icon3.png" mode=""></image>
					<view>购物车</view>
				</view>
			</view>
			<view class="payBtn" @click="webSelf.$Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">立即支付</view>
		</view>
	
		<!-- <view class="showSel" v-if="is_show" >
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
				<view class="flexRowBetween" style="padding: 20rpx 0; border-top: 2rpx solid #e7e7e7; border-bottom: 2rpx solid #e7e7e7; margin-top: 20rpx;">
					<view class="yy-title">购买数量</view>
					<view class="numBox" style="position: relative; right: auto; bottom: auto;">
						<view @click="reduce">-</view>
						<view class="num">{{proNum}}</view>
						<view @click="plus">+</view>
					</view>
				</view>
				<view class="submitbtn">
					<button type="submit" @click="webSelf.$Router.navigateTo({route:{path:'/pages/car/car'}})" >确定</button>
				</view>
			</view>
		</view> -->
	
	</view>

</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				mainData:{},
				indicatorDots: false,
				autoplay: false,
				interval: 5000,
				duration: 1000,
				
			}
		},

		onLoad(options) {
			const self = this;
			var options = self.$Utils.getHashParameters();
			if(options[0].id){
				self.id = options[0].id
			}
			self.$Utils.loadAll(['getMainData'], self);
		},

		onShow() {
			const self = this;
			document.title = '小镇市集'
		},

		methods: {
			
			addCart() {
				const self = this;
			
				if (JSON.stringify(self.mainData) == '{}') {
					
					self.$Utils.showToast('未选中商品', 'none');
					return;
				};
				self.mainData.count = 1;
				self.mainData.isSelect = true;
				var res = self.$Utils.setStorageArray('cartData', self.mainData, 'id', 999);
				if (res) {
					self.$Utils.showToast('加入成功', 'none');
				};
			},
			
			getMainData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
						id: self.id
					},
				};
				postData.order = {
					listorder:'desc'
				}
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					console.log('res', res)
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.productGet(postData, callback);
			},
			/* showSel(){
				const self = this;
				self.is_show = !self.is_show;
				
			},
			plus(proNum) {
				const self = this;
			    self.proNum++;
			},
			reduce(proNum){
				const self = this;
				if (self.proNum <= 1) {
					alert("不能少了")
				}else {
					self.proNum -= 1;
				}
			}, */

			
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/index.css";
	@import "../../assets/style/car.css";
	@import "../../assets/style/shopDetail.css";
	.swiper-box {
		height: 400rpx;
		margin: 0;
		border-radius: 0rpx !important;
	}
	
	
</style>
