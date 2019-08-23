<template>
	<view>
		<view class="caiLis flexRowBetween" v-for="(item,index) in mainData" :key="index">
			<view class="itemL">
				<view>
					<image src="../../static/images/shopping-icon1.png" v-if="item.isSelect" @click="choose(index)"></image>
					<image src="../../static/images/shopping-icon2.png" v-if="!item.isSelect" @click="choose(index)"></image>
				</view>
			</view>
			<view class="twoCt">
				<view class="leftbox">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				</view>
				<view class="cont">
					<view class="title avoidOverflow2">{{item.title}}</view>
					<view class="price">{{item.price}}</view>
					<view class="numBox">
						<view @click="counter(index,'-')">-</view>
						<view class="num">{{item.count}}</view>
						<view@click="counter(index,'+')">+</view>
					</view>
				</view>
			</view>
		</view>

		<view class="allPrice flexRowBetween">
			<view class="ll">
				<image @click="allsetBtn" v-show="isChooseAll" src="../../static/images/shopping-icon1.png"></image>
				<image @click="allsetBtn" v-show="!isChooseAll" src="../../static/images/shopping-icon2.png"></image>
				全选
			</view>
			<view class="rr">
				合计：<view class="mny">{{totalPrice}}</view>
				<span class="jsBtn">结算</span>
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
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/marketProdlist/marketProdlist'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">商品</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/car/car'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">购物车</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/myCenter/myCenter'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar4.png" />
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
				
				
				mainData:[]

			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			self.mainData = self.$Utils.getStorageArray('cartData');
			document.title = '购物车'
			console.log('self.mainData',self.mainData)
			self.checkChooseAll();
			self.countTotalPrice();
		},

		methods: {


			checkChooseAll() {
				const self = this;
				var isChooseAll = true;
				for (var i = 0; i < self.mainData.length; i++) {
					if (!self.mainData[i].isSelect) {
						isChooseAll = false;
					};
				};
				self.isChooseAll = isChooseAll;
			},

			chooseAll() {
				const self = this;
				self.isChooseAll = !self.isChooseAll;
				for (var i = 0; i < self.mainData.length; i++) {
					self.mainData[i].isSelect = self.isChooseAll;
					self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
				};
				self.countTotalPrice();
			},

		/* 	delete() {
				const self = this;
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						self.$Utils.delStorageArray('cartData', self.mainData[i], 'id');
					}
				};
				self.mainData = self.$Utils.getStorageArray('cartData');
				self.checkChooseAll();
				self.setData({
					web_mainData: self.mainData
				});
			}, */

			choose(index) {
				const self = this;
				
				if (self.mainData[index].isSelect) {
					self.mainData[index].isSelect = false;
				} else {
					self.mainData[index].isSelect = true;
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				
				self.checkChooseAll();
				self.countTotalPrice();
			},

			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						self.totalPrice += self.mainData[i].price * self.mainData[i].count;
					};
				};
			},



			pay(e) {
				const self = this;
				const orderList = [{
						product: [],
						type: 1
					}

				];
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						orderList[0].product.push({
							id: self.mainData[i].id,
							count: self.mainData[i].count,
							product:self.mainData[i]
						}, );
					};
				};
				if (orderList[0].product.length == 0) {
					self.$Utils.showToast('未选择商品', 'none', 1000);
					return;
				};
				uni.setStorageSync('payPro', orderList);
				self.$Utils.pathTo('/pages/oderTrue/oderTrue', 'nav')

			},


			counter(index,type) {
				const self = this;
				
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				
				self.countTotalPrice();
			},


		
		
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/car.css";

	page {
		padding-bottom: 160rpx;
		background: #f5f5f5;
	}
</style>
