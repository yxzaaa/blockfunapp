<template>
	<view class="container">
		<uni-background :src="imageLib.bg"/>
		<uni-nav-bar 
			title="我的好友" 
			textColor="#fff" 
			:opacity="scroll"
			:buttons="navButtons"
		/>
		<view class="app-container full">
			<view class="fun-card" style="margin:auto;">
				<view class="fun-card-item">
					<view style="font-size: 26upx;font-weight: bold;color:rgba(255,255,255,0.5);text-align: center;">好友数量</view>
					<view style="font-size: 36upx;font-weight: bold;color:rgba(255,255,255,1);text-align: center;padding-top: 20upx;">88</view>
				</view>
			</view>
			<view style="font-size: 26upx;font-weight: bold;width:750upx;color:rgba(255,255,255,0.5);text-align: center;padding-top:60upx;padding-bottom:30upx;border-bottom:2upx solid rgba(255,255,255,0.1);">好友明细</view>
			<view class="fortunetype">
				<view class="typeinfo">
					<view>
						<span class="type" style="width:30%">好友ID</span>
						<span class="type" style="width:30%">好友手机号</span>
						<span class="type" style="width:40%">好友注册日期</span>
					</view>
					<view v-for="(item,index) in coins" :key="index">
						<span class="numb" style="width:30%">{{item.unit}}</span>
						<span class="numb" style="width:30%">{{getNum(item.balance)}}</span>
						<span class="numb" style="width:40%">{{getNum(item.total)}}</span>
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
				navButtons:{
					back:{
						type:'normal',
						text:'取消'
					},		
				},
				imageLib:{
					bg:'../../static/bg2.jpg'
				},
			}
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
		width:750upx;
		padding:40upx 0upx;
		border-radius:8upx;
		view{
			display: flex;
			align-items: center;
			justify-content: space-between;
			.type{
				font-size: 26upx;
				color: #FFFFFF;			
				text-align: center;
			}
			.numb{
				font-style: normal;
				font-size: 26upx;
				padding-top:30upx; 
				color: #fff;
				text-align: center;
			}
		}
	}
</style>
