<template>
	<view>
		<view class="orderNav">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">待核销</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">已核销</view>
		</view>
		
		<view >
			<view class="prolisbox">
				<view class="prolis">
					<view class="datt">
						<view class="left">交易时间：2018-08-30</view>
						<view class="state">等待核销</view>
					</view>
					<view class="twoCt">
						<view class="leftbox">
							<image src="../../static/images/shopping-img1.png"></image>
						</view>
						<view class="cont">
							<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="price">59.00</view>
						</view>
					</view>
					<view class="flexRowBetween">
						<view class="" style="margin-top: 40rpx; font-size: 26rpx;">核销码：562345</view>
						<view class="bBtn">
							<view class="btn selt">确认核销</view>
						</view>
					</view>
					
				</view>
				
				<view class="prolis">
					<view class="datt">
						<view class="left">交易时间：2018-08-29</view>
						<view class="state">已核销</view>
					</view>
					<view class="twoCt">
						<view class="leftbox">
							<image src="../../static/images/shopping-img1.png"></image>
						</view>
						<view class="cont">
							<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="price">59.00</view>
						</view>
					</view>
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
				score: '',
				wx_info: {},
				current:1
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '我的预约'
		},

		methods: {
			change(current){
				const self = this;
				if(current!=self.current){
					self.current = current
				}
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
	@import "../../assets/style/myOrderList.css";
	.orderNav .tt{ width: 50%;}
	
</style>
