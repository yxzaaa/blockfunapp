<template>
	<view class="container">
		<uni-background />
		<uni-nav-bar :title="coin+' 账单'" textColor="#fff" :opacity="scroll" layout="center" :buttons="navButtons"></uni-nav-bar>
		<view class="app-container full">
			<view class="horizon-tab">
				<horizon-tab :tabs="navTabs" padding="50" @click="updateList"/>
			</view>
			<scroll-view class="order-box" scroll-y style="width:100%;height:calc(100vh - 274upx);" @scrolltolower="reachBottom">
				<view class="horizon-list">
					<view class="horizon-list-item" v-for="val in xdogList" :key="val.id">
						<view class="left-item">
							<text class="left-item-title">{{getDate(val.created_on)}}</text>
						</view>
						<view class="right-item">
							<span class="right-item-text" :style="{color:getColor(val.amount_unlock_balance)}">
								{{getNum(val.amount_unlock_balance)}}
							</span>
						</view>
					</view>
					<uni-load-more :status="loadStatus"></uni-load-more>
				</view>
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
					}
				},
				imageLib:{
					more:'../../static/icons/more.png'
				},
				navTabs:[
					{
						id:0,
						text:'全部'
					},
					{
						id:1,
						text:'转入'
					},
					{
						id:2,
						text:'转出'
					},
				],
				xdogList:[],
				coin:'',
				currPage:1,
				totalPage:1,
				loadStatus:'noMore'
			};
		},
		onLoad(option){
			this.coin = option.coin;
			this.updateList(0);
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		methods:{
			//上拉加载
			reachBottom(){
				if(this.currPage<this.totalPage){
					this.currPage++;
					this.loadStatus = 'loading';
					this.$http({
						url:'/v1/main/account/account-finance-record',
						data:{
							page:this.currPage,
							status:this.navTabs[this.currStatus].id,
							coin:this.coin,
							type:3,
							module:0
						},
						success:res=>{
							if(res.code == 200){
								this.loadStatus = 'more';
								this.xdogList = this.xdogList.concat(res.data.item);
							}
						}
					})
				}else{
					this.loadStatus = 'noMore';
				}
			},
			//更新转账记录
			updateList(index){
				uni.showLoading({
					title:'账单加载中...'
				})
				this.currStatus = index;
				this.currPage = 1;
				this.$http({
					url:'/v1/main/account/account-finance-record',
					data:{
						page:this.currPage,
						status:this.navTabs[index].id,
						coin:this.coin,
						type:3,
						module:0
					},
					success:res=>{
						if(res.code == 200){
							uni.hideLoading();
							this.xdogList = res.data.item;
							this.totalPage = res.data.max;
						}
					}
				})
			},
			toDetail(item){
				uni.setStorage({
					key:'trans_detail_info',
					data:item,
					success:res=>{
						uni.navigateTo({
							url:'../xdogdetail/xdogdetail'
						})
					}
				})
			},
			//获取交易号
			getTranId(tid){
				return tid.substr(0,5) + '***'+tid.substr(-9,9);
			},
			getNum(val){
				return parseFloat(val).toFixed(4)
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
			getColor(val){
				if(parseFloat(val)>0){
					return '#56CCF2'
				}else{
					return '#F2C94C'
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.order-box{
		width:750upx;
		height:calc(100vh - 268upx);
	}
	.horizon-list{
		padding:40upx 0upx;
		.horizon-list-item{
			padding:20upx 40upx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			.left-item{
				width:400upx;
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
				width:300upx;
				display: flex;
				justify-content: flex-end;
				align-items: center;
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
	}
</style>
