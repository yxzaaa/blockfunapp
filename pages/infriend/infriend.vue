<template>
	<view class="container">
		<uni-background src="../../static/bg1.jpg"/>
		<uni-nav-bar title="邀请好友" textColor="#fff" :opacity="scroll" layout="center" :buttons="navButtons"></uni-nav-bar>
		<view class="app-container full">
			<view class="fixed-buttons">
				<view class="button-group">
					<fun-button @handle="copy" value="复制链接" width="320upx" type="text" icon="../../static/icons/icon_lianjie.png"></fun-button>
					<span>|</span>
					<fun-button url="../initcard/initcard" value="生成邀请卡" width="320upx" type="text" icon="../../static/icons/icon_yaoqinka.png"></fun-button>
				</view>
			</view>
			<view style="padding:0upx 40upx;padding-bottom:150upx;">
				<view class="fun-card-item">
					<view class="lock-total-box">
						<view class="lock-item-box">
							<view class="lock-item">
								<text class="text-vertical-top">我的邀请码</text>
								<text class="text-vertical-bottom">{{friendCode}}</text>
							</view>
							<view class="lock-item-block" @click="copy">
								复制
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	import FunButton from '@/components/fun-button.vue';
	export default {
		components:{
			UniNavBar,
			UniBackground,
			FunButton
		},
		data() {
			return {
				scroll:0,
				navButtons:{
					back:{
						type:'normal',
						text:'取消'
					}
				},
				isVip:false,
				friendCode:'',
				imageLib:{
					alerm:'../../static/icons/icon_notification.png'
				}
			};
		},
		onLoad(){
			//获取邀请码
			uni.getStorage({
				key:'userInfo',
				success:res=>{
					this.friendCode = res.data.invite_code;
				}
			})
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		methods:{
			copy(){
				uni.setClipboardData({
					data:this.friendCode,
					success:()=>{
						uni.showToast({
							title:'邀请码已复制到剪贴板'
						})
					}
				})
			},
			openVip(){
				uni.showModal({
					title:'购买 SVIP 会员',
					content:'您即将购买SVIP会员，我们将从您的钱包中直接扣除199USDT，会员有效期一年。',
					success:res=>{
						if(res.confirm){
							
						}else if(res.cancel){
							
						}
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.fixed-buttons{
		bottom:40upx;
		width:670upx;
		left:40upx;
		border-radius: 10upx;
		padding:10upx;
		background: rgba(255,255,255,0.1);
		.button-group{
			display: flex;
			align-items: center;
			color:rgba(255,255,255,0.2);
			font-size: 28upx;
		}
	}
	.lock-total-box{
		width:100%;
		display:flex;
		justify-content:space-between;
		padding:20upx 0px;
		.lock-item-vertical{
			width:2upx;
			background:rgba(255,255,255,0.1);
			height:108upx;
		}
		.lock-item-box{
			width:100%;
			display:flex;
			justify-content:space-around;
			flex-wrap:wrap;
			.lock-item-block{
				margin:auto;
				width:160upx;
				border:1px solid rgba(255,255,255,0.5);
				border-radius: 8upx;
				text-align: center;
				font-size: 28upx;
				line-height: 52upx;
				margin-top:20upx;
				color:#c7c7c7;
				display:flex;
				justify-content:space-around;
				flex-wrap:wrap;
			}
			.lock-item{
				width:100%;
				text-align:center;
				text{
					display: block;
				}
			}
		}
	}
	.lockpay-box{
		width:750upx;
		background: #000;
		margin-bottom:30upx;
		.lock-table{
			padding:20upx 40upx;
			table{
				width:100%;
				border:none;
				thead{
					tr{
						th{
							border:none;
							text-align: center;
							line-height: 64upx;
							font-size: 28upx;
							color:#fff;
						}
					}
				}
				tbody{
					tr{
						td{
							border:none;
							text-align: center;
							line-height: 64upx;
							font-size: 28upx;
							color:#c7c7c7;
						}
					}
				}
			}
		}
		.lock-tab{
			border-bottom:2upx solid rgba(255,255,255,0.1);
			display:flex;
			justify-content:space-between;
			.lock-tab-item{
				width:100%;
				text-align:center;
				text{
					font-size:28upx;
					color:#fff;
					line-height:80upx;
					height:80upx;
					display: inline-block;
				}
				&.active text{
					border-bottom:2upx solid #fff;
				}
			}
		}
	}
	.text-vertical-top{
		font-size:28upx;
		color:#fff;
		opacity: 0.5;
	}
	.text-vertical-bottom{
		font-size:48upx;
		color:#fff;
		font-family: 'Montserrat-Light';
		line-height: 100upx;
	}
	.text-horizon-left{
		font-size:28upx;
		color:#fff;
		font-family: 'Montserrat-Bold';
		display: inline-block !important;
		padding:0upx 10upx;
	}
	.text-horizon-right{
		font-size:24upx;
		color:#999;
		font-family: 'Montserrat-Light';
		line-height: 48upx;
		display: inline-block !important;
	}
	.section-part{
		position: relative;
		padding-left:30upx;
		.list-num{
			position: absolute;
			line-height: 30upx;
			top:12upx;
			left:0upx;
		}
	}
	
</style>
