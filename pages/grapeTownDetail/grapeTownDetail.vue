<template>
	<view class="pro-detail">
		<view class="title">{{mainData.title}}</view>
		<view class="time">{{mainData.create_time}}</view>
		<view class="xqText">
			<view class="content ql-editor" v-html="mainData.content">
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				self: this,
				mainData:{}
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
			document.title = '太阳葡萄小镇'
		},


		methods: {

			

			getMainData() {
				const self = this;
				const postData = {
					thirdapp_id: 2,
					id:self.id
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					console.log('res', res)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/grapeTown.css";
</style>
