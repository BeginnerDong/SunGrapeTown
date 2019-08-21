<template>
	<view>
		<view class="fefuCont">
			<img class="phone" src="../../static/images/contact us-icon1.png" alt="">
			<view>客服电话</view>
			<view>029-89569965</view>
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
				wx_info: {}
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '客服'
		},

		methods: {
			change() {
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

.fefuCont{ width: 100%; margin-top: 300rpx; text-align: center; font-size: 30rpx; line-height: 50rpx; color: #666;}
.fefuCont .phone{ width: 140rpx; height: 140rpx;margin: 0 auto;}
</style>
