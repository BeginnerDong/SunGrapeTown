
<template>
	<view>
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
					<image src="../../static/images/nabar.png"/>
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png"/>
				</view>
				<view class="text  this-text">商品</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/car/car'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar3.png"/>
				</view>
				<view class="text">购物车</view>
			</view>	
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/myCenter/myCenter'}})">
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
	page{padding-bottom: 140rpx;}

</style>
