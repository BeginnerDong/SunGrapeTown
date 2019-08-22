<template>
	<view class="" >

		<view>
			<view class="mainbox" style=" border-bottom: 20rpx solid #f5f5f5;">
				<view class="twoCt">
					<view class="leftbox">
						<image src="../../static/images/yuyue-img1.png"></image>
					</view>
					<view class="cont">
						<view class="title avoidOverflow2">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
						<view class="price">59.00</view>
					</view>
				</view>
				<view class="flexRowBetween" style="padding-top: 20rpx; border-top: 2rpx solid #e7e7e7; margin-top: 30rpx;">
					<view class="yy-title">购买数量</view>
					<view class="numBox" style="position: relative; right: auto; bottom: auto;">
						<view @click="reduce">-</view>
						<view class="num">{{proNum}}</view>
						<view @click="plus">+</view>
					</view>
				</view>
			</view>
		</view>
		<view class="editLis">
			<view class="item flexRowBetween">
				<view class="lll">姓名</view>
				<view class="rrr">
					<input type="text" value="" placeholder="请输入姓名" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="lll">电话</view>
				<view class="rrr">
					<input type="number" value="" placeholder="请输入手机号"/>
				</view>
			</view>
			
			<view class="underFix">
				合计： <view class="price">56.00</view>
				<view class="btn">立即预约</view>
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
				score: '',
				proNum:1,
				wx_info: {}
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '下单预约'
		},

		methods: {
			plus(proNum) {
				const self = this;
			    self.proNum++;
			},
			reduce(proNum){
				const self = this;
				if (self.proNum <= 1) {
					alert("不能少了")
				}else {
					self.proNum -= 1;
				}
			},

			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			},
			showSel(){
				const self = this;
				self.is_show = !self.is_show;
				self.seltData({
					is_show:self.is_show
				})
			}
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
