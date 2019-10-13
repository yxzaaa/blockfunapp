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
			<horizon-tab :tabs="statusTabs" padding="42" @click="updateList" :active-tab.sync="currStatus"></horizon-tab>
			<!-- 收益 -->
			<scroll-view scroll-y class="earnbox" @scrolltolower="reachBottom"  @touchstart="start" @touchend="end">
				<view class="earning" v-for="(item,index) in walletList" :key="index">
					<view class="earn-text">
						<view class="earn-img" style="width:36upx;height:36upx;margin-right: 24upx;">
							<image src="../../static/icons/icon-1.png" style="width:36upx;height:36upx"></image>
						</view>
						<view class="earn-pro">
							<span style="color:#fff;font-size: 28upx;">{{item.remark}}</span>
							<span style="color:#fff;opacity: 0.5; font-size: 22upx;font-family: Montserrat-Bold">
								<span style="margin-right: 6upx;">{{getDate(item.created_on)}}</span>
							</span>
						</view>
					</view>
					<view class="earn-price" >
						<span v-if="item.amount_unlock_balance>0" style="color:#56CCF2;font-size: 36upx;font-weight: 600;font-family:Montserrat-Bold;margin-right:10upx;">{{getNum(item.amount_unlock_balance)}}</span>
						<span v-else style="color:#DA53A2;font-size: 36upx;font-weight: 600;font-family:Montserrat-Bold;margin-right:10upx;">{{getNum(item.amount_unlock_balance)}}</span>
						<!-- <image src="../../static/icons/more.png" style="width:40upx;height:40upx;"></image> -->
					</view>
				</view>
				<uni-load-more :status="loadStatus"></uni-load-more>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	import HorizonTab from '@/components/horizon-tab.vue';
	import UniLoadMore from '@/components/uni-load-more/uni-load-more.vue';
	export default {
		components:{
			UniNavBar,
			UniBackground,
			HorizonTab,
			UniLoadMore
		},
		data() {
			return {
				scroll:0,
				currStatus:0,
				navButtons:{
					back:{
						type:'normal',
						text:'取消'
					},		
				},
				statusTabs:[],
				currCoin:'',
				currPage:1,
				totalPage:0,
				walletList:[],
				loadStatus:'noMore',
				startData:{}
			}
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		onLoad(){
			//获取币种信息
			this.$http({
				url:"/v1/main/users/account-info",
				data:{
					type:1
				},
				success:res=>{
					res.data.map((item,index)=>{
						if(index == 0){
							this.currCoin = item.coin;
						}
						this.statusTabs.push({
							id:index+1,
							text:item.coin
						})
					})
					this.updateList(0)
				}
			})
		},
		methods: {
			reachBottom(){
				if(this.currPage<this.totalPage){
					this.currPage ++;
					this.loadStatus = "loading";
					this.$http({
						url:'/v1/main/account/account-finance-record',
						data:{
							type:1,
							status:0,
							coin:this.currCoin,
							page:this.currPage,
							module:0
						},
						success:res=>{
							console.log(res);
							if(res.code == 200){
								this.loadStatus = 'more';
								this.walletList = this.walletList.concat(res.data.item);
							}
						}
					})
				}else{
					this.loadStatus = 'noMore';
				}
			},
			updateList(index){
				this.currCoin = this.statusTabs[index].text;
				this.currPage = 1;
				uni.showLoading({
					title:"账单加载中..."
				})
				this.$http({
					url:'/v1/main/account/account-finance-record',
					data:{
						type:1,
						status:0,
						coin:this.currCoin,
						page:this.currPage,
						module:0
					},
					success:res=>{
						console.log(res);
						uni.hideLoading();
						this.walletList = res.data.item;
						this.totalPage = res.max;
					}
				})
			},
			start(e){
			    this.startData.clientX=e.changedTouches[0].clientX;        
			    this.startData.clientY=e.changedTouches[0].clientY;
			},
			end(e){
			    const subX=e.changedTouches[0].clientX-this.startData.clientX;
			    if(subX<-50){
					if(this.currStatus<this.statusTabs.length-1){
						this.currStatus++;
						this.updateList(this.currStatus);
					}
				}else if(subX>50){
					if(this.currStatus>0){
						this.currStatus--;
						this.updateList(this.currStatus);
					}
				}
			},
			getNum(num){
				return (parseFloat(num)).toFixed(2);
			},
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
		}
	}
</script>

<style lang="scss" scoped>
	.earnbox{
		width:750upx;
		height:calc(100vh - 268upx);
		.earning{
			margin:0px 40upx;
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
