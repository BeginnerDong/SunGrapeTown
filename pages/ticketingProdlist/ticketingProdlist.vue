<template>
	<view>
		<view class="proLis flexRowBetween">
			<view class="item-lis" v-for="(item,index) in mainData" :key="index" 
			@click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlistDetail/ticketingProdlistDetail?id='+item.id}})">
				<image class="img" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
				<view class="tit avoidOverflow2">{{item.title}}</view>
				<view class="price">{{item.price}}</view>
			</view>
		</view>


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
			document.title = '小镇市集'
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
						type: 2
					},
					paginate: self.$Utils.cloneForm(self.paginate)
				};
				postData.getBefore = {
					label: {
						tableName: 'Label',
						searchItem: {
							title: ['=', [self.title]],
						},
						middleKey: 'category_id',
						key: 'id',
						condition: 'in',
					},
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
					}
					console.log('res', res)
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.productGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/index.css";

	page {
		padding-bottom: 140rpx;
	}

	.proLis .item-lis {
		height: 480rpx;
	}

	.proLis .item-lis .tit {
		padding: 0;
		line-height: 40rpx;
		margin: 20rpx 4% 14rpx 4%;
	}

	.item-lis .img {
		width: 330rpx;
		height: 300rpx;
	}
</style>
