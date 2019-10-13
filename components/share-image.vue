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
				uni.canvasToTempFilePath({
					canvasId:'shareElem',
					success:res=>{
						console.log(res);
						this.tempFilePath = res.tempFilePath;
						uni.saveImageToPhotosAlbum({
							filePath:this.tempFilePath,
							success:res=>{
								uni.showToast({title:'图片已保存',icon:'none'})
							},
							fail:res=>{
								uni.showToast({title:'图片保存失败',icon:'none'})
							}
						})
					}
				})
			},
			share(data){
				uni.getImageInfo({
					src:data.img,
					success:res=>{
						let ctx = uni.createCanvasContext('shareElem',this);
						this.showModal = true;
						ctx.drawImage(res.path, 0, 0, 280,180);
						ctx.font = "16px Montserrat-Bold";
						ctx.fillStyle = "#DA53A2";
						ctx.fillText(data.price, 48, 210);
						ctx.font = "12px PingFang";
						ctx.fillStyle = "rgba(255,255,255,0.5)";
						ctx.fillText('USDT', 10, 210);
						ctx.fillStyle = "rgba(255,255,255,0.5)";
						// ctx.fillText('消耗积分', 10, 234);
						// ctx.fillStyle = "rgba(255,255,255,1)";
						// ctx.fillText(data.credit, 64, 234);
						ctx.fillStyle = "#DA53A2";
						ctx.fillRect(10,220,30,16);
						ctx.fillStyle = "rgba(255,255,255,1)";
						ctx.fillText(data.catname, 13, 232);
						ctx.font = "14px PingFang";
						ctx.fillStyle = "rgba(255,255,255,1)";
						ctx.fillText(data.title.substring(0,24), 10, 260);
						ctx.font = "14px PingFang";
						ctx.fillStyle = "rgba(255,255,255,1)";
						if(data.title.length>24){
							ctx.fillText(data.title.substring(24,42)+'...',10, 280);
						}
						ctx.font = "12px PingFang";
						ctx.fillStyle = "rgba(255,255,255,0.5)";
						ctx.fillText(data.content.substring(0,24)+'...',10, 310);
						ctx.fillStyle = "#95195C";
						ctx.fillRect(0,390,280,80);
						ctx.drawImage(data.qrcode, 8, 396, 50,50);
						ctx.font = "16px 'Montserrat-Bold'";
						ctx.fillStyle = "rgba(255,255,255,1)";
						ctx.fillText('BlockFun',70, 415);
						ctx.font = "14px 'Montserrat-Bold'";
						ctx.fillStyle = "rgba(255,255,255,0.5)";
						ctx.fillText('Unblock Grace',70, 440);
						ctx.draw();
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.modal-box{
		.canvas-box{
			width:280px;
			height:460px;
			background: #15030B;
			border-radius: 8px;
			border:8upx solid #fff;
			overflow:hidden;
			canvas{
				width:280px;
				height:460px;
			}
		}
	}
</style>
