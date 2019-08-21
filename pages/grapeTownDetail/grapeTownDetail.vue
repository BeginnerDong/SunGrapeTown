<template>
	<view class="pro-detail">
		<view class="title">标题标题标题标题标题标题标题标题标题标题</view>
		<view class="time">2019-8-20</view>
		<view class="xqText">
			<view>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
			<view>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
			<view>
				<image src="../../static/images/zhen1.png" mode="widthFix"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Self: this,
				submitData: {
					code: ''
				},
				sendCode: false,
				codeTimer: 0,
				subType: '',
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '太阳葡萄小镇'
		},


		methods: {

			change() {
				const self = this;
			},

			getProductData() {
				const self = this;
				const postData = {
					thirdapp_id: 2
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productData = res.info.data[0]

					}
					console.log('res', res)

					self.getCouponData()
				};
				self.$apis.productGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/grapeTown.css";
</style>
