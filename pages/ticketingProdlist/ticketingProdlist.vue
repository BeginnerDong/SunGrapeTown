<template>
	<view>
		<view class="proLis flexRowBetween">
			<view class="item-lis" v-for="(item,index) in produtList" :key="index" @click="webSelf.$Router.navigateTo({route:{path:'/pages/ticketingProdlistDetail/ticketingProdlistDetail'}})">
				<image class="img" :src="item.picUl" alt="" />
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
				showView: false,
				score: '',
				wx_info: {},
				produtList: [{
						picUl: "../../static/images/img01.png",
						title: "产品标题产品标题产品标题产品标题产品标题1",
						price: "56.00"
					},
					{
						picUl: "../../static/images/img01.png",
						title: "2产品标题产品标题产品标题产品标题产品标题2",
						price: "66.00"
					},
					{
						picUl: "../../static/images/img01.png",
						title: "3产品标题产品标题产品标题产品标题产品标题",
						price: "56.00"
					},
					{
						picUl: "../../static/images/img01.png",
						title: "4产品标题产品标题产品标题产品标题产品标题",
						price: "66.00"
					},
					{
						picUl: "../../static/images/img01.png",
						title: "5产品标题产品标题产品标题产品标题产品标题",
						price: "66.00"
					}
				]
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = '小镇市集'
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
