<template>
	<view>
		<view class="caiLis flexRowBetween">
			<view class="itemL">
				<view><image src="../../static/images/shopping-icon2.png" mode=""></image></view>
			</view>
			<view class="twoCt">
				<view class="leftbox">
					<image src="../../static/images/shopping-img1.png"></image>
				</view>
				<view class="cont">
					<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
					<view class="price">59.00</view>
					<view class="numBox">
						<view @click="plus">+</view>
						<view class="num">{{proNum}}</view>
						<view @click="reduce">-</view>
					</view>
				</view>
			</view>
		</view>
		<view class="caiLis flexRowBetween">
			<view class="itemL">
				<view><image src="../../static/images/shopping-icon1.png" mode=""></image></view>
			</view>
			<view class="twoCt">
				<view class="leftbox">
					<image src="../../static/images/shopping-img1.png"></image>
				</view>
				<view class="cont">
					<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
					<view class="price">59.00</view>
					<view class="numBox">
						<view>+</view>
						<view class="num">{{proNum}}</view>
						<view>-</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="allPrice flexRowBetween">
			<view class="ll">
				<image src="../../static/images/shopping-icon2.png" ></image>
				<image src="../../static/images/shopping-icon1.png" ></image>
				全选
			</view>
			<view class="rr">
				合计：<view class="mny">0.00</view>
				<span class="jsBtn">结算</span>
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
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/marketProdlist/marketProdlist'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png"/>
				</view>
				<view class="text">商品</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/car/car'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png"/>
				</view>
				<view class="text this-text">购物车</view>
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
				is_show:false,
				score: '',
				proNum:0,
				wx_info: {}
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '购物车'
		},

		methods: {
			change() {
				const self = this;
			},

			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			},
			plus(){
				const self = this;
				self.proNum = proNum++
			}
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/car.css";
	page{padding-bottom: 160rpx; background: #f5f5f5;}
	
	
	

</style>
