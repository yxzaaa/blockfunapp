<template>
	<view class="app-container">
		<uni-background />
		<uni-nav-bar 
			title="我的账单" 
			textColor="#fff" 
			:opacity="scroll"
			:buttons="navButtons"
		/>
		<view class="app-container full">
			<horizon-tab :tabs="statusTabs" padding="45"></horizon-tab>
			<!-- 收益 -->
			<view class="earnbox">
				<view class="earning">
					<view class="earn-text">
						<view class="earn-img" style="width:36upx;height:36upx;margin-right: 24upx;">
							<image src="../../static/icons/icon-1.png" style="width:36upx;height:36upx"></image>
						</view>
						<view class="earn-pro">
							<span style="color:#fff;font-size: 28upx;">推广收益</span>
							<span style="color:#fff;opacity: 0.5; font-size: 22upx;font-family: Montserrat-Bold">
								<span style="margin-right: 6upx;">2019/01/24</span>
								<span>01:22</span>
							</span>
						</view>
					</view>
					<view class="earn-price" >
						<span style="color:#DA53A2;font-size: 36upx;font-weight: 600;font-family:Montserrat-Bold;margin-right:10upx;">+8.88</span>
						<image src="../../static/icons/more.png" style="width:40upx;height:40upx;"></image>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	import HorizonTab from '@/components/horizon-tab.vue';
	export default {
		components:{
			UniNavBar,
			UniBackground,
			HorizonTab,
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
				statusTabs:[
					{id:1,text:'全部'},
					{id:2,text:'Forest'},
					{id:3,text:'Xdag'},
					{id:4,text:'USDT'},
				],
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
	.earnbox{
		width:100%;
		padding:0 30upx;
		.earning{
			padding-top:60upx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			.earn-text{
				display: flex;
				justify-content: space-between;
				.earn-pro{
					display: flex;
					flex-direction: column;
					span{
						line-height: 36upx;
					}
				}
			}
			.earn-price{
				display: flex;
				align-items: center;
			}
		}
	}
</style>
