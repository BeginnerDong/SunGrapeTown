
<template>
	<view class="container">
		<view class="town-lis" v-for="(item,index) in mainData" :key="index" @click="webself.$Router.navigateTo({route:{path:'/pages/grapeTownDetail/grapeTownDetail?id='+item.id}})">
			<view class="title avoidOverflow2">{{item.title}}</view>
			<view class="under">
				<view class="left">
					<view class="infor avoidOverflow3">{{item.description}}</view>
					<view class="data">{{item.create_time}}</view>
				</view>
				<view class="imgbox">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
				</view>
			</view>
		</view>
	</view>
</template>

 <script>
 	export default {
 		
 		data() {
 			return {
 				mainData:[],
 				webself:this,
 			
 			}
 		},
 		
 		onLoad() {		
 			const self = this;
 			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
 		
			if(options.title){
				self.title = options.title
			}else{
				self.$Utils.showToast('数据错误','none');
				return
			};
			
 			self.$Utils.loadAll(['getMainData'], self);			
 		},
		
		onShow() {
			const self = this;
			document.title = self.title
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
 			
 			
 			getMainData() {
 				const self = this;
 				const postData = {
 					searchItem:{
 						thirdapp_id:2
 					}			
 				};
 				postData.getBefore = {
 					article: {
 						tableName: 'Label',
 						searchItem: {
 							title: ['=', [self.title]],
 						},
 						middleKey: 'menu_id',
 						key: 'id',
 						condition: 'in',
 					},
 				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
 				console.log('postData', postData)
 				const callback = (res) => {
 					if (res.info.data.length > 0) {
 						self.mainData.push.apply(self.mainData, res.info.data);
 					} else {
 						self.$Utils.showToast('没有更多了','none');
 					};
 					console.log('res', res)
 					self.$Utils.finishFunc('getMainData');
 				};
 				self.$apis.articleGet(postData, callback);
 			},
 		},
 	};
 </script>



<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/grapeTown.css";

</style>
