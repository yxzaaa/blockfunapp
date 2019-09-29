<template>
	<view class="container">
		<uni-background :src="imageLib.bg"/>
		<uni-nav-bar 
			title="我的财务" 
			textColor="#fff" 
			:opacity="scroll"
			:buttons="navButtons"
		/>
		<view class="app-container full">
			<view style="margin:0upx 40upx;font-size:24upx;color:#fff;opacity: 0.5;">
				总资产
			</view>
			<view class="fortune" style="display: flex;justify-content: space-between;width:750upx;padding:0 40upx;margin-top: 16upx;align-items: center;">
				<span>
					<span style="font-size: 36upx;color:#fff;font-weight: bold;margin-right:10upx;">$</span>
					<span style="font-size: 60upx;color:#fff;font-family: Montserrat-Bold">{{totalAmount}}</span>
				</span>
				<span style="font-size:26upx;color:#fff;opacity: 0.5;">资产类型1种</span>
			</view>
			<view class="fortunetype">
				<view  style="font-size:28upx;color:#fff;margin:100upx 40upx 0;">
					资产类型：<span>USDT</span>
				</view>
				<view class="typeinfo">
					<view>
						<span class="type" style="width:30%">币种</span>
						<span class="type" style="width:30%">币种数量</span>
						<span class="type" style="width:40%">币种总价值($)</span>
					</view>
					<view v-for="(item,index) in coins" :key="index">
						<span class="numb" style="width:30%">{{item.unit}}</span>
						<span class="numb" style="width:30%">{{parseFloat(item.balance).toFixed(4)}}</span>
						<span class="numb" style="width:40%">{{parseFloat(item.total).toFixed(4)}}</span>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	export default {
		components:{
			UniNavBar,
			UniBackground,
		},
		data() {
			return {
				scroll:0,
				imageLib:{
					bg:'../../static/bg2.jpg'
				},
				navButtons:{
					back:{
						type:'normal',
						text:'取消'
					},
					textbtn:{
						text:'账单',
						url:'../bill/bill'
					}
				},
				coins:[],
				totalAmount:0
			}
		},
		onLoad(){
			//获取账号总资产
			uni.showLoading({
				title:'资产加载中',
			})
			this.$http({
				url:'/v1/main/users/account-finance',
				success:res=>{
					if(res.code == 200){
						uni.hideLoading();
						this.totalAmount = parseFloat(res.data.total).toFixed(4);
						this.coins = res.data.coin
					}
				}
			})
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
	.typeinfo{
		width:670upx;
		margin:20upx 40upx 50upx;
		padding:40upx 0upx;
		background: #2D1F25;
		border-radius:8upx;
		view{
			display: flex;
			align-items: center;
			justify-content: space-between;
			.type{
				font-size: 28upx;
				color: #FFFFFF;			
				opacity: 0.5;
				text-align: center;
			}
			.numb{
				font-style: normal;
				font-weight: 600;
				font-size: 32upx;
				padding-top:30upx;
				color: #fff;
				text-align: center;
			}
		}
	}
</style>
