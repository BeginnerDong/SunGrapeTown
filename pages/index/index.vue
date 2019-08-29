
<template>
	<view class="content">
		<view class="topWhite">
			<view>
				<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration" indicator-active-color="#db0160">
					<block v-for="(item,index) in labelData" :key="index">
						<swiper-item  class="swiper-item" >
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image"/>
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
			<view class="item-lis" v-for="(item,index) in mainData" :key="index"  
			@click="webSelf.$Router.navigateTo({route:{path:'/pages/marketProdDetail/marketProdDetail?id='+item.id}})">
				<image class="img" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt=""/>
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
			
				mainData:[],
				labelData:[],
				indicatorDots: false,
				autoplay: false,
				interval: 5000,
				duration: 1000
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getLabelData'], self);
		},

		onShow() {
			const self = this;
			document.title = '小镇市集'
		},

		methods: {
			change(){
				const self = this;
			},
			
			
			tokenGet() {
				const self = this;
				const postData = {
					searchItem: {
						user_no: 'U111111'
					}
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('user_token', res.token);
						uni.setStorageSync('user_no', res.info.user_no);
						uni.setStorageSync('user_info', res.info);
						uni.setStorageSync('token_expire_time',1565419588000)
					}
					console.log('res', res)
					self.$Utils.finishFunc('tokenGet');
				};
				self.$apis.tokenGet(postData, callback);
			},
			
			getLabelData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
					},
				};
				postData.getBefore = {
					parent: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['首页轮播']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data)
					}
					console.log('res', res)
					self.$Utils.finishFunc('getLabelData');
			
				};
				self.$apis.labelGet(postData, callback);
			},

			getMainData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
						type: 1
					},
					paginate:self.$Utils.cloneForm(self.paginate)
				};
				postData.order = {
					listorder:'desc'
				}
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
					}
					console.log('res', res)
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.productGet(postData, callback);
			},
		}
	}
 </script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/index.css";


</style>
