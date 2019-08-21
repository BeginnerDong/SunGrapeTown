<template>
	<view>
		<view ref="input" class="input" style="display: none;">  
		</view> 
	</view>
</template>

<script>
	export default {
		data(){
			return {
				currIndex: 0,
				hasChoose:false
			};
		},
		mounted() {  
			const self = this;
		    var input = document.createElement('input')  
		    input.type = 'file';
			input.accept = "";
		    input.id = 'custom-up';  
		    input.onchange = (event) => {
				
				this.ajax_upload(event);
				document.getElementById('custom-up').value = '';
		    };
		    this.$refs.input.$el.appendChild(input);  
		},
		props:{
		},
		methods: {
			customButtonClick(){
				const self = this;
				var btn = document.getElementById('custom-up');
				btn.click();
			},
			
			createXHR(){
				var xhr=null;
			   if(window.XMLHttpRequest)  //要是支持XMLHttpRequest的则采用XMLHttpRequest生成对象
				  xhr=new XMLHttpRequest();
			   else if(window.ActiveXobiect)//要是支持win的ActiveXobiect则采用ActiveXobiect生成对象。
				 xhr=new ActiveXobiect('Microsoft.XMLHTTP');
				return xhr;
			 },
			 ajax_upload(e){
			   const self = this;
			   var xhr = this.createXHR();
			   var formData=new FormData();
			   var file = e.target.files[0];
			   uni.showLoading();
			   var info = '文件名:'+file.name+' 文件类型:'+file.type+' 文件大小:'+file.size;
			   var newType = '';
			   console.log('file.type',file.type)
			   if(file.type=='image/png'||file.type=='image/jpg'||file.type=='image/jpeg'){
				   newType = 'image';
			   }else{
				   newType = 'file';
			   };
			   /* var showInfo=document.getElementById('showinfo');
			   var bar=document.getElementById('bar');
			   var progress=document.getElementById('progress');
			   showInfo.innerHTML= info; */
			   formData.append('file', file);
			   formData.append('token', uni.getStorageSync('user_token'));
			   var schedule = 0;
				//xhr.upload.onprogress是回调函数，接收参数d是ProgressEvent对象，d.loaded表示已经上传的，d.total表示文件总大小。
			   /* xhr.upload.οnprοgress=function(d){
				   progress.style.display='block';
				   schedule = d.loaded/d.total*100;
				   schedule = schedule.toFixed(2);
				   console.log(d);
				   bar.style.width = schedule+'%';
				   bar.innerHTML = schedule+'%';
			   }; */
			   xhr.open('POST', 'http://loan.52team.top/api/public/index.php/api/v1/Base/FtpFile/upload', true);
			   xhr.send(formData);
			   xhr.onreadystatechange = function(){
				 if( this.readyState == 4 && this.status == 200){
					var res = JSON.parse(this.responseText);
					if(res.solely_code==100000){
						self.$emit('uploadAfter', {
							url:res.info.url,
							type:newType,
							o_type:file.type,
							name:file.name
						});
					}else if(res.solely_code==200000){
						self.$Router.navigateTo({route:{path:'/pages/index/index'}});
					}else{
						uni.showToast({
							title: '上传失败',
							duration: 1000,
							success:function(){
								
							}
						});
					};
					self.hasChoose = false;
					uni.hideLoading();
					/* showInfo.innerHTML = showInfo.innerHTML+'<br />'+this.responseText;
					progress.style.display = 'none'; */
				 }
			   }
			   document.getElementById('custom-up').value = '';
			 },
		}
	}
	
</script>

<style>
</style>
