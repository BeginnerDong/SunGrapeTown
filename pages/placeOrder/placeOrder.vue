<template>
	<view class="" >

		<view>
			<view class="mainbox" style=" border-bottom: 20rpx solid #f5f5f5;">
				<view class="twoCt">
					<view class="leftbox">
						<image src="../../static/images/yuyue-img1.png"></image>
					</view>
					<view class="cont">
						<view class="title avoidOverflow2">
						{{mainData[0]&&mainData[0].sku&&mainData[0].sku[0]&&mainData[0].sku[0].product?mainData[0].sku[0].product.title:''}}
						</view>
						<view class="price">{{mainData[0]&&mainData[0].sku&&mainData[0].sku[0]&&mainData[0].sku[0].product?mainData[0].sku[0].product.price:''}}</view>
					</view>
				</view>
				<view class="flexRowBetween" style="padding-top: 20rpx; border-top: 2rpx solid #e7e7e7; margin-top: 30rpx;">
					<view class="yy-title">购买数量</view>
					<view class="numBox" style="position: relative; right: auto; bottom: auto;">
						<view @click="counter('-')">-</view>
						<view class="num">{{mainData[0]&&mainData[0].sku&&mainData[0].sku[0]?mainData[0].sku[0].count:''}}</view>
						<view @click="counter('+')">+</view>
					</view>
				</view>
			</view>
		</view>
		<view class="editLis">
			<view class="item flexRowBetween">
				<view class="lll">姓名</view>
				<view class="rrr">
					<input type="text" value="" placeholder="请输入姓名" v-model="name"/>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="lll">电话</view>
				<view class="rrr">
					<input type="number" value="" placeholder="请输入手机号" v-model="phone"/>
				</view>
			</view>
			
			<view class="underFix">
				合计： <view class="price">{{totalPrice}}</view>
				<view class="btn"  @click="webSelf.$Utils.stopMultiClick(submit)">立即预约</view>
			</view>
			
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				addressData:{},
				mainData:[],
				name:'',
				phone:''
			}
		},

		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
			self.mainData = self.$Utils.jsonToArray(uni.getStorageSync('payProTwo'), 'unshift');
			console.log('self.data.mainData', self.mainData);
			self.countTotalPrice();
		},

		onShow() {
			const self = this;
			document.title = '下单预约'
		},

		methods: {
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				if(self.name==''||self.phone==''){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请填写姓名或电话','none')
				}else{
					self.addOrder()
				}
			},
			
			addOrder() {
				const self = this;					
				const postData = {
					tokenFuncName: 'getProjectToken',
					orderList: self.mainData,
					data:{
						name:self.name,
						phone:self.phone,
						passage1:self.mainData[0].sku[0].standard
					}
				};	
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay(self.orderId)
					} else {		
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.addOrder(postData, callback);
			},
			
			pay(order_id) {
				const self = this;
				
				const postData = {};	
				postData.wxPay = {
					price: self.totalPrice
				};
				/* postData.score = self.totalPrice; */
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.orderId
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/ticketingCenter/ticketingCenter'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/ticketingCenter/ticketingCenter'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			
			
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.mainData[0].sku[0].count++;
				} else {
					if (self.mainData[0].sku[0].count > 1) {
						self.mainData[0].sku[0].count--;
					}
				};			
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;				
				for (var i = 0; i < self.mainData[0].sku.length; i++) {
					self.totalPrice += self.mainData[0].sku[i].skuPrice * self.mainData[0].sku[i].count;		
				};
			},
			
			
		}
	}
</script>


<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/index.css";
	@import "../../assets/style/car.css";
	@import "../../assets/style/shopDetail.css";
	
	.editLis{margin: 30rpx 4%;box-shadow: 0 0 10rpx rgba(0,0,0,0.1); border-radius: 10rpx;}
	.editLis .item{height: 100rpx;padding: 0 4%; }
	.editLis .item:first-child{ border-bottom: 2rpx solid #e7e7e7;}
	.editLis .item .lll{ font-size: 28rpx; width: 30%;}
	.editLis .item .rrr{ width: 70%; }
	.editLis .item .rrr input{ width: 100%; padding: 0 20rpx; box-sizing:border-box ; font-size: 28rpx; text-align: right;}

	.underFix{ width: 100%; padding: 0 4%;box-sizing: border-box; line-height: 100rpx; box-shadow: 0 -4rpx 6rpx rgba(0,0,0,0.1); display: flex; justify-content: flex-end; font-size: 28rpx; align-items: center; position: fixed; bottom: 0; left: 0;}
	.underFix .price{ font-size: 28rpx; color: #C6112B; font-weight: bold;}
	.underFix .price::before{content: "￥"; font-weight: normal;}
	.underFix .btn{ width: 200rpx; height: 80rpx; text-align: center; line-height: 80rpx; color: #fff; background: #C6112B; border-radius: 50rpx; margin-left: 20rpx;}
</style>
