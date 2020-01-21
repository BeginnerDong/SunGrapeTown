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
		<view class="detailTit">
			<view class="flexRowBetween">
				<view class="spxq " style="margin-bottom: 0;">选择日期</view>
				<view class="Rmore" @click="showSel">查看更多&gt;</view>
			</view>
			<view class="seltDatabox flexRowBetween">
				<view class="mm" @click="chooseSku(index)" v-for="(item,index) in mainData.sku" :style="chooseId==item.id?'border:none;background:#c6112b;color:#fff':''">{{item.title}}</view>	
				<view class="mm"  @click="showSel">...</view>
			</view>
		</view>
		<view class="xqInfor">
			<view class="spxq">商品详情</view>
			<view class="cont">
				<view class="content ql-editor" v-html="mainData.content">
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
						<view class="title avoidOverflow2">{{mainData.title}}</view>
						<view class="price">{{mainData.price}}</view>
					</view>
				</view>
				<view class="yy-title" style="padding: 20px 0 10px 0;">日期</view>
				<view class="yyDate-lis flexRowBetween" style="flex-wrap: wrap; justify-content: flex-start;">
					<view class="mm" @click="chooseSku(index)" v-for="(item,index) in mainData.sku" :style="chooseId==item.id?'border:none;background:#c6112b;color:#fff':''">{{item.title}}</view>	
				</view>
				<view class="flexRowBetween" style="padding: 30rpx 0; border-top: 2rpx solid #e7e7e7; border-bottom: 2rpx solid #e7e7e7; margin-top: 10rpx;">
					<view class="yy-title">购买数量</view>
					<view class="numBox" style="position: relative; right: auto; bottom: auto;">
						<view @click="counter('-')">-</view>
						<view class="num">{{count}}</view>
						<view @click="counter('+')">+</view>
					</view>
				</view>
				<view class="submitbtn">
					<button type="submit" @click="goBuy" >下单预约</button>
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
				indicatorDots: false,
				autoplay: false,
				interval: 5000,
				duration: 1000,
				mainData:{},
				chooseId:'',
				count:1,
				orderList:[]
				
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
			document.title = '商品详情'
		},

		methods: {
			
			

			getMainData() {
				const self = this;
				var start = new Date(new Date().toLocaleDateString()).getTime()/1000;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
						id: self.id
					},
				};
			
				postData.getAfter = {
					sku:{
						order:{
							standard:'asc'
						},
						tableName:'Sku',
						middleKey:'product_no',
						key:'product_no',
						searchItem:{
							status:1,
							standard:['>=',start]
						},
						condition:'='
					}
				};
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
			
			chooseSku(index) {
				const self = this;				
				
				self.count = 1;
				self.chooseId = self.mainData.sku[index].id;
				self.skuPrice = self.mainData.sku[index].price;
				self.stock = self.mainData.sku[index].stock,
				self.standard = self.mainData.sku[index].standard
				self.type = self.mainData.type
				/* self.$Router.navigateTo({route:{path:'/pages/placeOrder/placeOrder'}}) */
			},
			
			counter(type) {
				const self = this;
				if(!self.type){				
					self.$Utils.showToast('请选择预定日期','none');
					return
				};
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};
				uni.setStorageSync('payProTwo', self.orderList);
			},
			
			goBuy(){
				const self = this;
				if(self.chooseId==''){
					self.$Utils.showToast('请选择预定日期','none');
					return
				};
				if (self.stock<self.count) {
					self.$Utils.showToast('当日库存不足', 'none');
					return;
				};
				self.orderList = [{
					sku: [{
						id:self.chooseId,
						count: self.count,
						product:self.mainData,
						skuPrice:self.skuPrice,
						standard:self.standard
					}],
					type: self.type,
				}];
				uni.setStorageSync('payProTwo', self.orderList);
				self.$Router.navigateTo({route:{path:'/pages/placeOrder/placeOrder'}})
			},
			
			showSel(){
				const self = this;
				self.is_show = !self.is_show;
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
