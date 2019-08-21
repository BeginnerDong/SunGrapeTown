<template>
<view class="container">
	
	<view class="nav2" >
		<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/grapeTown/grapeTown'}})">有机种植</view>
		<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/grapeTown/grapeTown'}})">美酒佳酿</view>
		<view class="child" @click="webSelf.$Router.navigateTo({route:{path:'/pages/grapeTown/grapeTown'}})">亲子营地</view>
	</view>
	
	<!-- 底部菜单 -->	
	<view class="under-navbar" >
		<view class="nav">关于小镇</view>
		<view class="nav" @click="webSelf.$Router.navigateTo({route:{path:'/pages/index/index'}})">小镇市集</view>
		<view class="nav" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketing/ticketing'}})">票务预定</view>
	</view>

</view>

</template>

<script>
	export default {
		data() {
			return {
				webSelf: this,
				score:'',
				isShow:false
			}
		},

		onLoad(options) {
			const self = this;

		},
		onShow() {
			const self = this;
			document.title = '进入首页'			
		},


		methods: {
			showAlert(){
				this.showView = true;
			},
			coloseAlert(){
				this.showView = false;
			},

			getMainData() {
				const self = this;
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						if (self.mainData.hasOrder.length == 0) {
							self.is_show = true;
							
						} else {

							self.$Router.redirectTo({
								route: {
									path: '/pages/todayread/todayread'
								}
							})
						}
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	.under-navbar{height: 60rpx;padding: 10px 0;display: flex;align-items: center;text-align: center;font-size: 24rpx;color: #333;background: #e7e7e7; position: fixed;bottom: 0;left: 0; right: 0;}
	.under-navbar .nav{width: 33.3%;border-right: 1px solid #ccc;}
	.under-navbar .nav:last-child{border-right: none;}
	
	.nav2{width: 33.3%;background-color: #e7e7e7; position: fixed; bottom: 100rpx;left: 0;}
	.nav2 .child{width: 100%; line-height: 80rpx;height: 80rpx;text-align: center; border-bottom: 1px solid #ccc;font-size: 28rpx; color: #666;}
	
</style>
