<template>
	<view>
		<view>
			<view class="meallLis" v-for="(item,index) in mainData" :key="index" 
			@click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlistDetail/ticketingProdlistDetail?id='+item.id}})">
				<view class="twoCt">
					<view class="leftbox">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
					</view>
					<view class="cont">
						<view class="title avoidOverflow2">{{item.title}}</view>
						<view class="price">{{item.price}}</view>
					</view>
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/ticketing/ticketing'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar1.png" />
				</view>
				<view class="text">门票预订</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/ticketingMeals/ticketingMeals'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar2-a.png" />
				</view>
				<view class="text  this-text">餐食预订</view>
			</view>
			<view class="navbar_item" @click="webSelf.$Router.redirectTo({route:{path:'/pages/ticketingCenter/ticketingCenter'}})">
				<view class="nav_img">
					<image src="../../static/images/1nabar3.png" />
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
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			/* var options = self.$Utils.getHashParameters(); */
			
			self.title = '餐饮预订';
			self.$Utils.loadAll(['getMainData'], self);
		},

		onShow() {
			const self = this;
			document.title = '餐食预约'
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
			change() {
				const self = this;
			},

			getMainData() {
				const self = this;
				const postData = {
					searchItem: {
						thirdapp_id: 2,
						type: 3
					},
					paginate: self.$Utils.cloneForm(self.paginate)
				};
			
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
					}
					console.log('res', res)
					self.$Utils.finishFunc('getMainData')		
				};
				self.$apis.productGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/car.css";

	page {
		padding-bottom: 140rpx;
		background: #f5f5f5;
	}
	.navbar .navbar_item{ width: 33.3%;}
	
	.meallLis{ margin:30rpx 4% 0 4%; background: #fff; border-radius: 10rpx;padding: 30rpx;}
	.meallLis .twoCt{ width: 100%;}
	.meallLis .twoCt .leftbox{ width: 180rpx; height: 180rpx;}
	.meallLis .twoCt .cont{ width: 420rpx; margin-left: 20rpx;}
</style>
