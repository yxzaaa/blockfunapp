<template>
	<view class="container">	
		<uni-background /> <!-- 背景色-->
		<!-- 导航栏 -->
		<uni-nav-bar 	
			title="支付订单"
			textColor="#fff"
			:opacity="scroll"
			:buttons="navButtons"
		/>
		<view class="app-container full">
			<view class="payNumber">
				<view class="symbolNumber">
					<span class="price">{{amountCount}}</span>
					<span class="symbol">{{currCoin}}</span>
				</view>
			</view>
			<view class="payStyle">
				<view class="cash" v-for="(item,index) in walletList" :key="index" @click="toggleCoin(index)">
					<span class="content">
						{{item.coin}}
					</span>
					<span class="number">
						<span>
							{{item.amount}}
						</span>
						<image v-if="currCoin != item.coin"  src="../../static/bg/checkbox.png" style="width:32upx;height:32upx;margin-left:16upx;"></image>
						<image v-if="currCoin == item.coin"  src="../../static/bg/check.png" style="width:32upx;height:32upx;margin-left:16upx;"></image>
					</span>
				</view>
			</view>
			<view class="fixed-buttons">
				<view class="button-group">
					<fun-button :value="'支付 '+amountCount+' '+currCoin" width="670upx" large @handle="showModal = true"></fun-button>
				</view>
			</view>
			<view class="modal-box" v-if="showModal" @touchmove.stop.prevent="showModal">
				<view class="modal" v-if="!jump">
					<view class="modal-top-item">
						<view class="modal-title">请输入您的支付密码</view>
						<view class="modal-content">
							<possword-inputer @input="setPassword" :value="payPassword"></possword-inputer>
						</view>
					</view>
					<view class="modal-btns">
						<view @click="showModal = false">取消</view>
						<view style="border-left:1px solid #eee;color:#0A61C9;" @click="payOrder">支付</view>
					</view>
				</view>
				<view class="modal" v-if="jump">
					<view class="modal-top-item">
						<view class="modal-title" style="padding:0upx;color:#DA53A2;">账户余额不足&nbsp;&nbsp;{{timeOut}}秒后前往转入</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import FunButton from '@/components/fun-button.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import PosswordInputer from '@/components/possword-inputer.vue';
	export default {
		components:{
			UniNavBar,
			UniBackground,
			FunButton,
			PosswordInputer
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
				amountCount:'',
				orderId:'',
				showModal:false,
				payPassword:'',
				walletList:[],
				currCoin:'USDT',
				timeOut:3,
				jump:false,
				timer:null
			}
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		onLoad(){
			uni.getStorage({
				key:'submit_order_result',
				success:res=>{
					this.orderId = res.data.id;
					this.amountCount = res.data.amount;
					this.walletList = res.data.wallet;
				}
			})
		},
		methods: {
			//切换币种
			toggleCoin(index){
				this.currCoin = this.walletList[index].coin;
				this.amountCount = this.walletList[index].amount;
			},
			//设置密码
			setPassword(val){
				this.payPassword = val;
			},
			//订单支付
			payOrder(){
				console.log(this.payPassword);
				if(this.payPassword.length === 8){
					this.$http({
						url:'/order/pay',
						type:'application/x-www-form-urlencoded',
						data:{
							item:this.orderId,
							password:this.payPassword,
							coin:this.currCoin
						},
						success:res=>{
							if(res.code == 200){
								this.showModal = false;
								uni.navigateTo({
									url:'../pay-result/pay-result?id='+this.orderId
								})
							}else{
								if(res.message == '账户余额不足' || res.code == '300004'){
									this.jump = true;
									this.timer = setInterval(()=>{
										if(this.timeOut>1){
											this.timeOut --;
										}else{
											clearInterval(this.timer);
											this.jump = false;
											this.timeOut = 3;
											this.showModal = false;
											uni.navigateTo({
												url:'../comein/comein?coin='+this.currCoin+'&type=3'
											})
										}
									},1000)
								}else{
									uni.showToast({
										title:res.message,
										icon:'none'
									})
								}
							}
						}
					})
				}else{
					uni.showToast({
						title:"请输入8位支付密码",
						icon:'none'
					})
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.payNumber{
		width:750upx;
		padding:80upx 0upx;
		padding-top:60upx;
		display:flex;
		justify-content:center;
		.symbol{
			font-size:32upx;
			color:rgba(255,255,255,0.5);
			font-weight: 600;
			margin-left:20upx;
		}
		.price{
			font-size:56upx;
			font-family:'Montserrat-Bold';
			color:#DA53A2;
			font-weight: 600;
		}
	}
	.payStyle{
		width:670upx;
		background:#2D1F25;
		padding:20upx 40upx;
		border-radius: 8upx;
		margin:auto;
		display: flex;
		flex-direction: column;
		align-content:start;
		.cash{
			display: flex;
			justify-content: space-between;
			padding:15upx 0upx;
		}
		.number{
			
			display: flex;
			align-items: center;
			span{
				font-size:28upx;
				color:#DA53A2;
				font-weight:bold;
				font-family:'Montserrat-Bold';
			}
		}
		.integral{
			display: flex;
			justify-content: space-between;
			margin-top:50upx;
		}
		.content{
			color:#fff;
			font-size: 28upx;
		}
	}
	.button-group{
		width:670upx;
	}
</style>
