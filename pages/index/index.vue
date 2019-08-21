
<template>
	<view class="content">
		<view class="topWhite">
			<view>
				<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration" indicator-active-color="#db0160">
					<block v-for="(item,index) in imgUrls" :key="index">
						<swiper-item  class="swiper-item" >
							<image :src="item" class="slide-image"/>
						</swiper-item>
					</block> 
				</swiper>
			</view>
			<view class="infor-title">
				<view class="tt">主打商品</view>
				<view class="titLine"></view>
			</view>
		</view>
		
		<view class="proLis flexRowBetween">
			<view class="item-lis" v-for="(item,index) in produtList" :key="index"  @click="webSelf.$Router.navigateTo({route:{path:'/pages/marketProdDetail/marketProdDetail'}})">
				<image class="img" :src="item.picUl" alt=""/>
				<view class="tit avoidOverflow">{{item.title}}</view>
				<view class="price">{{item.price}}</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item"  @click="webSelf.$Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png"/>
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/marketProdlist/marketProdlist'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png"/>
				</view>
				<view class="text">商品</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/car/car'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar3.png"/>
				</view>
				<view class="text">购物车</view>
			</view>	
			<view class="navbar_item"  @click="webSelf.$Router.redirectTo({route:{path:'/pages/myCenter/myCenter'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar4.png"/>
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
				showView: false,
				score:'',
				wx_info:{},
				produtList:[
					{
						picUl:"../../static/images/hone-img.png",
						title:"产品标题1",
						price:"56.00"
					},
					{
						picUl:"../../static/images/hone-img.png",
						title:"产品标题2",
						price:"66.00"
					},
					{
						picUl:"../../static/images/hone-img.png",
						title:"产品标题1",
						price:"56.00"
					},
					{
						picUl:"../../static/images/hone-img.png",
						title:"产品标题2",
						price:"66.00"
					},
					{
						picUl:"../../static/images/hone-img.png",
						title:"产品标题2",
						price:"66.00"
					}
				],
				imgUrls: [
					  '../../static/images/home-banner.png',
					  '../../static/images/home-banner.png',
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
			change(){
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


</style>
