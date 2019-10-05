<template>
	<view class="container">
		<uni-background :src="imageLib.bg"/>
		<uni-nav-bar 
			title="" 
			textColor="#fff" 
			:opacity="scroll"
			:buttons="navButtons"
		/>
		<view class="app-container full">
			<view class="card-title">
				<image src="../../static/title.png"></image>
			</view>
			<view class="card-title" style="padding-top:240upx;">
				<image :src="imgQrCode" style="width:360upx;height:360upx;padding:30upx;background: #fff;border-radius: 10upx;"></image>
			</view>
			<view class="card-title" style="padding-top:0px;">
				<image src="../../static/share_bg.png" style="width:210upx;height:36upx;"></image>
			</view>
		</view>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	import QRCode from '@/components/wxqrcode.js';
	export default {
		components:{
			UniNavBar,
			UniBackground,
		},
		data() {
			return {
				scroll:0,
				navButtons:{
					back:{
						type:'normal',
						text:'取消'
					},		
				},
				imageLib:{
					bg:'../../static/bg/lockbill.jpg'
				},
				friendCount:0,
				friends:[],
				imgQrCode:''
			}
		},
		onLoad(){
			//获取我的好友列表
			this.$http({
				url:'/v1/main/users/my-friends',
				success:res=>{
					if(res.code == 200){
						this.friendCount = res.data.count;
						this.friends = res.data.friends;
						this.qrcode();
					}
				}
			})
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		methods: {
			getDate(timestamp){
				var date = new Date(timestamp*1000);
				var year = date.getFullYear();
				var month = date.getMonth() + 1;
				var day = date.getDate();
				var hour = date.getHours();
				var min = date.getMinutes();
				month = month>=10?month:'0'+month;
				day = day>=10?day:'0'+day;
				hour = hour>=10?hour:'0'+hour;
				min = min>=10?min:'0'+min;
				return year+'/'+month+'/'+day+' '+hour+':'+min
			},
			qrcode () {
			  this.imgQrCode = QRCode.createQrCodeImg('https://blockfuntest.dm1.in/h5/html/index.html', {  
			       size: parseInt(100)//二维码大小  
			  })
			},
		}
	}
</script>

<style lang="scss" scoped>
	.card-title{
		width:750upx;
		padding:80upx 0upx;
		image{
			width:620upx;
			height:60upx;
			display: block;
			margin:auto;
		}
	}
</style>
