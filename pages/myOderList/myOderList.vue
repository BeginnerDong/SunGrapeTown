<template>
	<view>
		<view class="orderNav">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">待支付</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">待发货</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">待收货</view>
			<view class="tt" :class="current==5?'on':''" @click="change('5')">已完成</view>
		</view>
		<view class="prolisbox">
			<view class="prolis" v-for="(item,index) in mainData">
				<view class="datt">
					<view class="left">交易时间：{{item.create_time}}</view>
					<view class="state" v-if="item.pay_status==0">等待支付</view>
					<view class="state" v-if="item.pay_status==1&&item.transport_status==0">等待发货</view>
					<view class="state" v-if="item.pay_status==1&&item.transport_status==1">等待收货</view>
					<view class="state" v-if="item.pay_status==1&&item.order_step==3">已完成</view>
				</view>
				<view class="twoCt" v-for="c_item in item.products">
					<view class="leftbox">
						<image :src="c_item.snap_product&&c_item.snap_product.mainImg&&c_item.snap_product.mainImg[0]?c_item.snap_product.mainImg[0].url:''"></image>
					</view>
					<view class="cont">
						<view class="title avoidOverflow2">{{c_item.snap_product?c_item.snap_product.title:''}}</view>
						<view class="price">{{c_item.snap_product?c_item.snap_product.price:''}}</view>
					</view>
				</view>
				<view class="bBtn">
					<view class="btn gopay" v-if="item.pay_status==0" @click="pay(item.id,item.price)">去支付</view>
					<view class="btn selt" v-if="item.pay_status==0" @click="orderUpdate(index,'1')">取消订单</view>
					<view class="btn" v-if="item.pay_status==1&&item.transport_status==1" @click="orderUpdate(index,'2')">确认收货</view>
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
				searchItem:{
					
				},
				mainData:[],
				current:1
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			var res = self.$Token.getProjectToken(function(){
				self.$Utils.loadAll(['getMainData'], self)
			});
			if(res){
				self.$Utils.loadAll(['getMainData'], self)
			};
		},

		onShow() {
			const self = this;
			document.title = '我的订单'
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},

		methods: {
			
			pay(order_id,price) {
				const self = this;
				const postData = {};	
				postData.wxPay = {
					price: price
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: order_id
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
										self.mainData = [];
										self.paginate={
											count: 0,
											currentPage:1,
											pagesize:5,
											is_page:true,
										};
										self.getMainData()
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
								self.mainData = [];
								self.paginate={
									count: 0,
									currentPage:1,
									pagesize:5,
									is_page:true,
								};
								self.getMainData()
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
			
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
					self.searchItem = {}
					if (self.current == '1') {
					
					} else if (self.current == '2') {
						self.searchItem.pay_status = '0';
					
					} else if (self.current == '3') {
						self.searchItem.pay_status = '1';
						self.searchItem.transport_status = '0';
					} else if (self.current == '4') {
						self.searchItem.pay_status = '1';
						self.searchItem.transport_status = '1';
					} else if (self.current == '5') {
						self.searchItem.pay_status = '1';
						self.searchItem.transport_status = '2';
					};
					self.mainData = [];
					self.paginate={
						count: 0,
						currentPage:1,
						pagesize:5,
						is_page:true,
					};
					self.getMainData()
				}
			},

			getMainData() {
				const self = this;		
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem.type = 1;
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了','none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			orderUpdate(index,type) {
				const self = this;		
				const postData = {};			
				postData.searchItem = {
					order_no:self.mainData[index].order_no
				};		
				if(type=='1'){
					postData.data = {
						status:-1
					};
				}else{
					postData.data = {
						transport_status:2
					};
				}
				postData.tokenFuncName = 'getProjectToken';			
				const callback = (res) => {
					if(res.solely_code==100000){
						if(type=='1'){
							self.$Utils.showToast('取消成功','none',1000);
						}else{
							self.$Utils.showToast('确认成功','none',1000);
						}					
						setTimeout(function() {
							self.mainData = [];
							self.paginate={
								count: 0,
								currentPage:1,
								pagesize:5,
								is_page:true,
							};
							self.getMainData()
						}, 1000);
						
					}else{
						self.$Utils.showToast(res.msg,'none');
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/myOrderList.css";
</style>
