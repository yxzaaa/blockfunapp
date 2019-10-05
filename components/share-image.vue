<template>
	<!-- 分享图片 -->
	<view class="modal-box" v-show="showModal" @touchmove.stop.prevent="showModal">
		<view class="canvas-box">
			<canvas canvas-id="shareElem"></canvas>
		</view>
		<view class="fixed-buttons">
			<view class="button-group">
				<fun-button value="取消分享" width="320upx" type="light" large @handle="showModal= false"></fun-button>
				<fun-button value="保存图片" width="320upx" large @handle="saveImage()"></fun-button>
			</view>
		</view>
	</view>
</template>

<script>
	import FunButton from '@/components/fun-button.vue';
	export default {
		components:{
			FunButton
		},
		data() {
			return {
				showModal:false,
				tempFilePath:''
			};
		},
		methods:{
			saveImage(){
				uni.saveImageToPhotosAlbum({
					filePath:this.tempFilePath,
					success : (res) =>{
						uni.showToast({title : '图片已保存',icon:'none'})
					}
				})
			},
			share(data){
				uni.getImageInfo({
					src:data.img,
					success:res=>{
						let ctx = uni.createCanvasContext('shareElem');
						this.showModal = true;
						uni.showLoading({
							title:'生成分享图片 ...'
						})
						setTimeout(()=>{
							uni.hideLoading();
							ctx.drawImage(res.path, 0, 0, 250,180);
							ctx.font = "16px Montserrat-Bold";
							ctx.fillStyle = "#DA53A2";
							ctx.fillText(data.price, 48, 210);
							ctx.font = "12px PingFang";
							ctx.fillStyle = "rgba(255,255,255,0.5)";
							ctx.fillText('USDT', 10, 210);
							ctx.fillStyle = "rgba(255,255,255,0.5)";
							ctx.fillText('消耗积分', 10, 234);
							ctx.fillStyle = "rgba(255,255,255,1)";
							ctx.fillText(data.credit, 64, 234);
							ctx.fillStyle = "#DA53A2";
							ctx.fillRect(10,245,30,16);
							ctx.fillStyle = "rgba(255,255,255,1)";
							ctx.fillText(data.catname, 13, 257);
							ctx.font = "14px PingFang";
							ctx.fillStyle = "rgba(255,255,255,1)";
							ctx.fillText(data.title.substring(0,24), 10, 280);
							ctx.font = "14px PingFang";
							ctx.fillStyle = "rgba(255,255,255,1)";
							ctx.fillText(data.title.substring(24,40)+'...',10, 300);
							ctx.font = "12px PingFang";
							ctx.fillStyle = "rgba(255,255,255,0.5)";
							ctx.fillText(data.content.substring(0,20)+'...',10, 325);
							ctx.fillStyle = "#95195C";
							ctx.fillRect(0,340,250,110);
							ctx.drawImage(data.qrcode, 8, 346, 40,40);
							ctx.font = "16px 'Montserrat-Bold'";
							ctx.fillStyle = "rgba(255,255,255,1)";
							ctx.fillText('BlockFun',60, 362);
							ctx.font = "14px 'Montserrat-Bold'";
							ctx.fillStyle = "rgba(255,255,255,0.5)";
							ctx.fillText('Unblock Grace',60, 382);
							ctx.draw(false,()=>{
								uni.canvasToTempFilePath({
									canvasId:'shareElem',
									success: (res) =>{
										this.tempFilePath = res.tempFilePath;
									}
								})
							})
						},500)
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.modal-box{
		.canvas-box{
			width:240px;
			height:400px;
			background: #15030B;
			border-radius: 8px;
			border:8upx solid #fff;
			overflow:hidden;
			canvas{
				width:240px;
				height:400px;
			}
		}
	}
</style>
