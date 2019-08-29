<template>
	<view class="content">
		<view class="linelis">
			<view class="left-tit">收货人</view>
			<view class="right-ct">
				<input type="text" placeholder="请输入真实姓名" v-model="submitData.name">
			</view>
		</view>	
		<view class="linelis">
			<view class="left-tit">手机号码</view>
			<view class="right-ct" >
				<input style="text-align: right;" type="text" placeholder="+86" v-model="submitData.phone">
			</view>
		</view>	
		<view class="linelis">
			<view class="left-tit">所在地区</view>
			<view class="right-ct" style="font-size:14px;color: #666666;" @click="showMulLinkageThreePicker">
				{{submitData.city==''?'请选择地区':submitData.city}}
				<!-- <input style="text-align: right;" type="text" placeholder="省市区"> -->
			</view>
		</view>	
		<view class="linelis" style="min-height: 100rpx; height: auto;">
			<view class="left-tit">详细地址</view>
			<view class="right-ct" >
				<textarea class="textarea"  bindblur="bindTextAreaBlur"  placeholder="如街道/门牌号" v-model="submitData.detail"></textarea>
			</view>
		</view>	
		<view class="linelis">
			<view class="left-tit" style="width: 50%;">设为默认地址</view>
			<view class="right-ct" style="width:50%;text-align: right" >
				<switch @change="choose" :checked="submitData.isdefault==1?true:false" color="#c6112b"></switch>
				<!-- <img class="mmIcon" src="../../static/images/address-icon02.png" alt="">
				<img v-if="isshow" class="mmIcon" src="../../static/images/address-icon03.png" alt=""> -->
			</view>
		</view>	
		
		<view class="submitbtn" @click="webSelf.$Utils.stopMultiClick(submit)">
			<button class="radiu20 color5">保存</button>
		</view>
	
		<mpvue-city-picker :themeColor="themeColor" ref="mpvueCityPicker" :pickerValueDefault="cityPickerValueDefault"
		 @onCancel="onCancel" @onConfirm="onConfirm"></mpvue-city-picker>
	</view>
</template>

<script>
	import mpvuePicker from '../../components/mpvue-picker/mpvuePicker.vue';
	// https://github.com/zhetengbiji/mpvue-citypicker
	import mpvueCityPicker from '../../components/mpvue-citypicker/mpvueCityPicker.vue'
	import cityData from '../../common/city.data.js';

	export default {
		components: {
			mpvuePicker,
			mpvueCityPicker
		},
		
		
		data() {
			return {
				submitData: {
					name: '',
					city: '',
					detail: '',
					isdefault:0
				},
				webSelf:this,
				mulLinkageTwoPicker: cityData,
				cityPickerValueDefault: [0, 0, 0],
				themeColor: '#F98A48',
				
				mode: '',
				deepLength: 1,
				pickerValueDefault: [0],
				pickerValueArray:[],
				
			}
		},
		onLoad(options) {
			const self = this;
			if(options.id){
				self.id = options.id;
				var res = self.$Token.getProjectToken(function(){
					self.$Utils.loadAll(['getMainData'], self)
				});
				if(res){
					self.$Utils.loadAll(['getMainData'], self)
				};
			}
		},
		
		onShow() {
			const self = this;
			document.title = '添加地址'
		},
		
		methods: {
			
			showMulLinkageThreePicker() {
				this.$refs.mpvueCityPicker.show()
			},
			onConfirm(e) {
				
				this.submitData.city = e.label;
				console.log('e',e)
			},
			onCancel(e){
				console.log('e',e)
			},
			
			
			
			choose(e) {
				const self = this;
				if(e.target.value){
					self.submitData.isdefault = 1
				}else{
					self.submitData.isdefault = 0
				}
				console.log('switch2 发生 change 事件，携带值为', e.target.value)
			},

			getMainData(id) {
				const self = this;
				const postData = {};
				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					console.log(res);
					
					self.submitData.name = res.info.data[0].name;
					self.submitData.detail = res.info.data[0].detail;
					self.submitData.city = res.info.data[0].city;
					self.submitData.phone = res.info.data[0].phone;
					self.submitData.isdefault = res.info.data[0].isdefault;
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},



			addressUpdate() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);

				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('修改成功','success');
						self.$Router.redirectTo({route:{path:'/pages/address/address'}})
					} else {
						self.$Utils.showToast(data.msg,'error')
					};
					
				};
				self.$apis.addressUpdate(postData, callback);
			},


			addressAdd() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);

				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('添加成功','success');
						self.$Router.redirectTo({route:{path:'/pages/myAddress/myAddress'}})
					} else {
						self.$Utils.showToast(data.msg,'success')
					}
					
				};
				self.$apis.addressAdd(postData, callback);
			},


			submit() {
				const self = this;
				
				var phone = self.submitData.phone;
				const pass = self.$Utils.checkComplete(self.submitData);

				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					
					if (self.id) {

						self.addressUpdate();
					} else {

						self.addressAdd();
					}

			
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息','success');
				};
			},

		}
	}
</script>

<style>
	@import "../../assets/style/common.css";
	.content{ padding: 0 4%;}
	.linelis{  height: 100rpx; display: flex; justify-content: space-between; align-items: center; border-bottom: 2rpx solid #e7e7e7;}
	.linelis .left-tit{ width: 20%; font-size: 28rpx; color: #333; line-height: 60rpx;}
	.right-ct{ width: 80%;}
	.right-ct input{ width: 100%; height: 60rpx ; line-height: 60rpx; padding: 0 20rpx; box-sizing: border-box; font-size: 28rpx;}
	.right-ct .textarea{ width: 100%; height: 100rpx!important; padding:10rpx 20rpx;font-size: 28rpx; display: block; box-sizing: border-box;  min-height:60rpx;padding-top: 34rpx;}
	.mmIcon{ width: 120rpx;  height: 50rpx; display:inline-block;}
</style>
