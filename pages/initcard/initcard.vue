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
				imgQrCode:''
			}
		},
		onLoad(option){
			this.qrcode(option.code);
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		methods: {
			qrcode (code) {
				var url = 'https://blockfuntest.dm1.in/h5/html/index.html?conn='+code;
				this.imgQrCode = QRCode.createQrCodeImg(url, {  
				    size: parseInt(200)//二维码大小  
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
