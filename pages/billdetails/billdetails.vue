<template>
	<view class="container">
		<uni-background src="../../static/bg1.jpg"/>
		<uni-nav-bar title="挂单详情" textColor="#fff" :opacity="scroll" layout="center" :buttons="navButtons"></uni-nav-bar>
		<div class="app-container full fixbutton" style="padding-bottom:190upx;">
			<view class="modal-box" v-if="showModal">
				<view class="modal">
					<view class="modal-top-item">
						<view class="modal-title">预计放款金额</view>
						<view class="modal-content">
							<view style="font-size: 24upx;padding-top:10upx;text-align: left;">1.质押率：USDT 的质押率为 50%，即放款比例为本金的50%；</view>
							<view style="font-size: 24upx;padding-top:10upx;text-align: left;">2.补仓线：USDT 的补仓线为 75%，抵押市值到达 75% 后，会通知借款方补仓；</view>
							<view style="font-size: 24upx;padding-top:10upx;text-align: left;">3.平仓线：USDT 的平仓线为 60%，抵押市值到达 60% 后，投资人有权进行平仓；</view>
						</view>
					</view>
					<view class="modal-btns">
						<view style="border-left:1px solid #eee;color:#0A61C9;width:100%;" @click="showModal = false">关闭</view>
					</view>
				</view>
			</view>
			<view class="user-count" style="padding-top:20upx;">
				<text style="font-family: 'Montserrat-Bold';font-size:64upx;">{{details.total}}</text>
				<text>USDT</text>
			</view>
			<view class="user-info" @click="showInfo">
				<text style="color:rgba(255,255,255,0.5);font-size: 26upx;">预计放款金额</text>
				<image style="width:36upx;height:36upx;margin-left:8upx;" :src="imageLib.info"></image>
			</view>
			<view style="padding:40upx;">
				<view class="fun-card">
					<view class="fun-card-item">
						<view class="horizon-list-item">
							<view class="left-item">
								<text class="left-item-label">交易类型</text>
							</view>
							<view class="right-item">
								<text class="left-item-name">{{details.type == 2?'借贷挂单':'投资挂单'}}</text>
							</view>
						</view>
						<view class="horizon-list-item">
							<view class="left-item">
								<text class="left-item-label">抵押币种</text>
							</view>
							<view class="right-item">
								<view class="right-item">
									<text class="left-item-name">{{details.unit}}</text>
								</view>
							</view>
						</view>
						<view class="horizon-list-item">
							<view class="left-item">
								<text class="left-item-label">USDT价格</text>
							</view>
							<view class="right-item">
								<text class="left-item-name">{{currUnitPrice}} {{details.unit == 'USDT'?'USDT':'USDT/'+details.unit}}</text>
							</view>
						</view>
						<view class="horizon-list-item">
							<view class="left-item">
								<text class="left-item-label">抵押单价</text>
							</view>
							<view class="right-item">
								<text class="left-item-name">{{details.price}}</text>
							</view>
						</view>
						<view class="horizon-list-item">
							<view class="left-item">
								<text class="left-item-label">抵押总量</text>
							</view>
							<view class="right-item">
								<text class="left-item-name">{{details.locked}}</text>
							</view>
						</view>
						<!-- <view style="width:100%;height:3upx;background:rgba(255,255,255,0.2);margin:20upx 0upx;"></view> -->
						<view class="horizon-list-item">
							<view class="left-item">
								<text class="left-item-label">抵押周期</text>
							</view>
							<view class="right-item">
								<text class="left-item-name">{{details.month}}个月</text>
							</view>
						</view>
						<view class="horizon-list-item">
							<view class="left-item">
								<text class="left-item-label">月利率</text>
							</view>
							<view class="right-item">
								<text class="left-item-name">{{details.rate*100}}%</text>
							</view>
						</view>
						<view class="horizon-list-item">
							<view class="left-item">
								<text class="left-item-label">预计利息</text>
							</view>
							<view class="right-item">
								<text class="left-item-name">{{details.interest}} {{details.unit}}</text>
							</view>
						</view>
						<view class="horizon-list-item">
							<view class="left-item">
								<text class="left-item-label">手续费</text>
							</view>
							<view class="right-item">
								<text class="left-item-name">{{details.fee}} {{details.unit}}</text>
							</view>
						</view>
					</view>
				</view>
			</view>
		</div>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	import FunButton from '@/components/fun-button.vue';
	import PasswordInputer from '@/components/possword-inputer.vue';
	export default {
		components:{
			UniNavBar,
			UniBackground,
			FunButton,
			PasswordInputer
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
				imageLib:{
					info:'../../static/icons/icon_info.png',
					sanjiao:'../../static/icons/sanjiao.png',
					certCheck:'../../static/icons/cert_check.png',
					certChecked:'../../static/icons/cert_checked.png',
				},
				showModal:false,
				details:{},
				currUnitPrice:0
			};
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		onLoad(){
			//获取当前挂单详情
			uni.getStorage({
				key:'my_bill_detail',
				success:res=>{
					console.log(res.data);
					this.details = res.data;
					//请求币种和参考价格
					this.$http({
						url:'/v1/main/debit/debit-preloading',
						success:res=>{
							if(res.code == 200){
								console.log(res.data);
								res.data.coin.map(item=>{
									if(item.coin == this.details.unit){
										this.currUnitPrice = item.unit_price;
									}
								});
							}
						}
					})
				}
			})
		},
		methods:{
			showInfo(){
				this.showModal = true;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.radio-box{
		border-radius: 10upx;
		overflow: hidden;
		margin:0px;
		text{
			border-radius: 0px;
		}
	}
	.symble{
		margin-left:10upx;
		color:#999;
	}
	.user-info{
		width:230upx;
		margin:auto;
		display:flex;
		justify-content:center;
		align-items:center;
		padding:10upx 0;
		image{
			width:48upx;
			height:48upx;
		}
		text{
			color:#fff;
			font-size: 32upx;
		}
	}
	.user-count{
		width:750upx;
		text-align:center;
		padding:10upx 0;
		text{
			color:#fff;
			font-size: 44upx;
			padding:0px 10upx;
		}
	}
	.erweima{
		width:200upx;
		height:200upx;
		background: #fff;
		border-radius: 12upx;
		margin:auto;
		padding:20upx;
		image{
			width:100%;
			height:100%;
		}
	}
	.horizon-list-item{
		padding:16upx 0upx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		.left-item{
			display:flex;
			justify-content:flex-start;
			align-items:center;
			.left-item-avatar{
				width:40upx;
				height:40upx;
				margin-right:20upx;
			}
			.left-item-name{
				color:#fff;
				font-size: 26upx;
				padding-right:15upx;
			}
			.left-item-label{
				color:#999;
				font-size: 24upx;
				margin-right: 18upx;
			}
			.left-item-title{
				display: block;
				color:#fff;
				font-size: 32upx;
				line-height: 52upx;
				font-family:'Montserrat-Bold';
				span{
					font-family:'Montserrat-Bold';
				}
			}
			.left-item-date{
				display: block;
				color:#999;
				font-size: 26upx;
				line-height: 52upx;
				font-family:'Montserrat-Light';
			}
		}
		.right-item{
			display:flex;
			justify-content:flex-end;
			align-items:center;
			color:#999;
			font-size: 24upx;
			.left-item-name{
				color:#fff;
				font-size: 26upx;
			}
			.right-item-text{
				font-size: 38upx;
				color:#56CCF2;
				font-family:'Montserrat-Bold';
				span{
					font-family:'Montserrat-Bold';
				}
			}
			image{
				width:42upx;
				height:42upx;
				margin-left:10upx;
			}
		}
	}
</style>
