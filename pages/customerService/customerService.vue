<template>
	<view>
		<view class="fefuCont" @click="callPhone()">
			<img class="phone" src="../../static/images/contact us-icon1.png" alt="">
			<view>客服电话</view>
			<view>{{mainData.description}}</view>
		</view>

	</view>

</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				showView: false,
				score: '',
				mainData: {}
			}
		},

		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);		
		},
		
		onShow() {
			const self = this;
			document.title = '客服'
		},
		
		methods: {
			
			callPhone(){
				const self = this;
				uni.makePhoneCall({
				    phoneNumber: self.mainData.description
				});
			},
			
			
			getMainData() {
				const self = this;	
				const postData = {
					thirdapp:2,
					searchItem:{
						
						title:'客服电话'
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
				self.$apis.labelGet(postData, callback);
			},
		}
	}
</script>

<style>
@import "../../assets/style/common.css";

.fefuCont{ width: 100%; margin-top: 300rpx; text-align: center; font-size: 30rpx; line-height: 50rpx; color: #666;}
.fefuCont .phone{ width: 140rpx; height: 140rpx;margin: 0 auto;}
</style>
