
<template>
	<view class="container">
		<view class="town-lis" v-for="(item,index) in townLis" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/grapeTownDetail/grapeTownDetail'}})">
			<view class="title avoidOverflow2">{{item.title}}</view>
			<view class="under">
				<view class="left">
					<view class="infor avoidOverflow3">{{item.infor}}</view>
					<view class="data">{{item.time}}</view>
				</view>
				<view class="imgbox">
					<image :src="item.imgUrl" mode=""></image>
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
				showView: false,
				score:'',
				login_name:'',
				wx_info:{},
				townLis:[
					{
						title:"标题1标题1标题1标题1标题1标题1标题1标题1标题1标题1",
						infor:"内容1内容1内容1内容1内容1内容1内容1内容1内容1内容1内容1内容1内容1",
						time:"2019-8-20",
						imgUrl:"../../static/images/zhen1.png"
					},
					{
						title:"标题2标题2标题2标题2标题2标题2标题2标题2标题2",
						infor:"内容22222内容22222内容22222内容22222内容22222内容22222内容22222内容22222",
						time:"2019-8-20",
						imgUrl:"../../static/images/zhen1.png"
					}
				]
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
			
			change(){
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
	@import "../../assets/style/grapeTown.css";

</style>
