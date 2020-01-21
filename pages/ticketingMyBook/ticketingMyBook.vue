<template>
	<view>
		<view class="orderNav">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">待核销</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">已核销</view>
		</view>
		
		<view >
			<view class="prolisbox">
				<view class="prolis" v-for="(item,index) in mainData">
					<view class="datt">
						<view class="left">交易时间：{{item.create_time}}</view>
						<view class="state" v-if="item.transport_status==0">等待核销</view>
						<view class="state" v-if="item.transport_status==2">已核销</view>
					</view>
					<view class="twoCt" v-for="c_item in item.products">
						<view class="leftbox">
							<image :src="c_item.snap_product&&c_item.snap_product.product&&c_item.snap_product.product.mainImg&&c_item.snap_product.product.mainImg[0]?c_item.snap_product.product.mainImg[0].url:''"></image>
						</view>
						<view class="cont">
							<view class="title avoidOverflow2">{{c_item.snap_product&&c_item.snap_product.product?c_item.snap_product.product.title:''}}</view>
							<view class="price">{{c_item.snap_product&&c_item.snap_product.product?c_item.snap_product.product.price:''}}</view>
						</view>
					</view>
					<view class="flexRowBetween" v-if="item.transport_status==0">
						<view class="" style="margin-top: 40rpx; font-size: 26rpx;">核销码：{{item.express_info}}</view>
						<view class="bBtn" @click="submit(index)">
							<view class="btn selt">确认核销</view>
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
				searchItem:{
					transport_status:0
				},
				current:1,
				mainData:[]
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			var options = self.$Utils.getHashParameters();
			if(options[0].id){
				self.id = options[0].id
			}
			var res = self.$Token.getProjectToken(function(){
				self.$Utils.loadAll(['getMainData'], self)
			});
			if(res){
				self.$Utils.loadAll(['getMainData'], self)
			};
		},

		onShow() {
			const self = this;
			document.title = '我的预约'
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
			
			submit(index){
				const self = this;
				var today = new Date(new Date().toLocaleDateString()).getTime()/1000;
				console.log('today',today)
				self.index = index;
				
			
				if(today!=self.mainData[self.index].passage1&&today>self.mainData[self.index].passage1){
				
					self.$Utils.showToast('订单已过期','none')
				}else if(today!=self.mainData[self.index].passage1&&today<self.mainData[self.index].passage1){
					
					self.$Utils.showToast('未到预约时间','none')
				}else{
					self.orderUpdate()
				}
			},
			
			change(current){
				const self = this;
				if(current!=self.current){
					self.current = current
					if(self.current==1){
						self.searchItem = {
							transport_status:0
						}
					}else{
						self.searchItem = {
							transport_status:2
						}
					};
					self.mainData = [];
					self.paginate={
						count: 0,
						currentPage:1,
						pagesize:5,
						is_page:true,
					};
					self.getMainData()
				}
			},

			getMainData() {
				const self = this;		
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.pay_status = 1;
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem.type = self.id;
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了','none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			
			
			orderUpdate() {
				const self = this;		
				const postData = {};			
				postData.searchItem = {
					order_no:self.mainData[self.index].order_no
				};
				postData.data = {
					transport_status:2
				};
				postData.tokenFuncName = 'getProjectToken';			
				const callback = (res) => {
					if(res.solely_code==100000){
						self.$Utils.showToast('核销成功','none',1000);
						setTimeout(function() {
							self.mainData = [];
							self.paginate={
								count: 0,
								currentPage:1,
								pagesize:5,
								is_page:true,
							};
							self.getMainData()
						}, 1000);
						
					}else{
						self.$Utils.showToast(res.msg,'none');
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
			
		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	@import "../../assets/style/myOrderList.css";
	.orderNav .tt{ width: 50%;}
	
</style>
