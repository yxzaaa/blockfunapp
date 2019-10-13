<template>
	<view class="container">
		<uni-background />
		<uni-nav-bar
			title="订单管理" 
			textColor="#fff" 
			:opacity="scroll"
			:buttons="navButtons"
		/>
		<view class="app-container full">
			<!-- 顶部滑动 -->
			<horizon-tab :tabs="statusTabs" padding="45" @click="toggleStatus" :active-tab.sync="currStatus" ></horizon-tab>
			<scroll-view class="order-box" scroll-y @scrolltolower="reachBottom" @touchstart="start" @touchend="end">
				<view class='empty-box' v-if="orderList.length == 0">
					<image src="../../static/icons/empty_order.png" style="width:420upx;height:200upx;"></image>
					<text>您没有相关订单</text>
				</view>
				<view v-if="orderList.length !== 0" class="managebox" v-for="(val,index) in orderList" :key="index"> <!-- 待办管理 -->
					<view class="backlog" @click="goDetail(val.id)"> 
						<span>{{val.order}}</span>
						<view style="font-size:28upx" :style="{color:getStatus(val.status).color }">{{getStatus(val.status).name}}</view>
					</view>
					<view class="goodslist" @click="goDetail(val.id)">
						<view class="goods" v-for="(val1,index1) in val.item" :key="index1">
							<view class="image">
								<image :src="val1.img" style="width:160upx;height:160upx;display: block;"></image>
							</view>
							<view class="text">
								<view style="color:#fff;font-size:26upx;width:470upx;height:66upx;line-height: 34upx;display: block;">{{val1.title.length>40?val1.title.substring(0,40)+' ...':val1.title}}</view>
								<view style="overflow-x:auto;white-space: nowrap;width:470upx;display: flex;align-items: center;padding-top:14upx;">
									<span style="color: #999999;font-size:24upx;margin-right:24upx;">数量：{{val1.number}}</span>
									<span style="color: #999999;font-size:24upx;">{{val1.p1}}：{{val1.s1}}</span>
									<span style="color: #999999;font-size:24upx;margin-left:24upx;" v-if="val1.p2">{{val1.p2}}：{{val1.s2}}</span>
								</view>
								<view style="white-space: nowrap;width:470upx;display: flex;align-items: center;padding-top:8upx;">
									<span style="color:#fff;font-size:28upx;font-family: Montserrat-Bold;">{{setPrice(val1.price,val1.number)}}</span>
									<span style="font-size:24upx;display: inline-block;font-family:'Montserrat-Bold';color:rgba(255,255,255,0.5);margin-left:10upx;">USDT</span>
								</view>
							</view>
						</view>
					</view>
					<view class="account">
						<span style="color:#fff;font-size: 24upx;opacity: 0.7;margin-right:24upx;">
							共 <span style="margin:0 6upx;"> {{getTotalNum(val.item)}} </span> 件
						</span>
						<span style="color:#fff;font-size: 28upx;">
							合计<span style="color:#DA53A2;font-family: Montserrat-Bold;margin-left:12upx;font-size: 34upx;">
								{{val.amount}}
								<span style="font-size:24upx;display: inline-block;font-family:'Montserrat-Bold';color:rgba(255,255,255,0.5);margin-left:10upx;">USDT</span>
							</span>
						</span>
					</view>
					<view class="button-group" v-if="val.status == 1 || val.status == 3">
						<fun-button @handle="cancelOrder(val.id)" class="funbtn1" value="取消订单" width="200upx" background="rgba(41,26,33,0.6)" color="#999" v-if="val.status == 1"></fun-button>
						<fun-button @handle="goPayOrder(val.id,val.amount)" class="funbtn1" value="去支付" width="200upx" v-if="val.status == 1"></fun-button>
						<fun-button @handle="confirmReceipt(val.id)" class="funbtn1" value="确认收货" width="200upx" v-if="val.status == 3"></fun-button>
					</view>
				</view>
				<uni-load-more v-if="orderList.length !== 0" :status="loadStatus"></uni-load-more>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	import HorizonTab from '@/components/horizon-tab.vue';
	import FunButton from '@/components/fun-button.vue';
	import UniLoadMore from '@/components/uni-load-more/uni-load-more.vue';
	export default {
		components:{
			UniNavBar,
			UniBackground,
			HorizonTab,
			FunButton,
			UniLoadMore
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
				statusTabs:[
					{id:-1,text:'全部',color:''},
					{id:1,text:'待付款',color:'#DA53A2'},
					{id:2,text:'待发货',color:'#F2C94C'},
					{id:3,text:'待收货',color:'#56CCF2'},
					{id:4,text:'已完成',color:'#999999'},
					{id:8,text:'已取消',color:'#999999'},
					{id:9,text:'已关闭',color:'#999999'},
				],
				orderList:[],
				currStatus:0,
				currPage:1,
				totalPage:1,
				loadStatus:'noMore',
				startData:{}
			}
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		onLoad(){
			this.toggleStatus(0);
		},
		methods: {
			//上拉触底
			reachBottom(){
				if(this.currPage<this.totalPage){
					this.currPage ++;
					this.loadStatus = 'loading';
					this.$http({
						url:'/member/order?nav='+this.statusTabs[this.currStatus].id+'&page='+this.currPage,
						success:res=>{
							if(res.code == 200){
								this.loadStatus = 'more';
								this.orderList = this.orderList.concat(res.data.item);
							}
						}
					})
				}else{
					this.loadStatus = 'noMore';
				}
			},
			toggleStatus(index){
				this.currStatus = index;
				this.currPage = 1;
				uni.showLoading({
					title:'订单加载中...'
				})
				this.$http({
					url:'/member/order?nav='+this.statusTabs[index].id+'&page='+this.currPage,
					success:res=>{
						if(res.code == 200){
							uni.hideLoading();
							this.orderList = res.data.item;
							this.totalPage = res.data.max;
							this.loadStatus = 'noMore';
						}
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
						this.toggleStatus(this.currStatus);
					}
				}else if(subX>50){
					if(this.currStatus>0){
						this.currStatus--;
						this.toggleStatus(this.currStatus);
					}
				}
			},
			//确认收货
			confirmReceipt(id){
				uni.showModal({
					title:'确认收货？',
					content:'确认收货后货款将转到商家账户！',
					success:(res)=>{
						if (res.confirm) {
							this.$http({
								url:'/order/receive?id='+id,
								success:res=>{
									if(res.code == 200){
										this.toggleStatus(this.currStatus);
									}
								}
							})
						}
					}
				})
			},
			goPayOrder(id,amount){
				uni.navigateTo({
					url:'../pay-order/pay-order?id='+id+'&amount='+amount
				})
			},
			cancelOrder(id){
				uni.showModal({
					title:'取消订单？',
					content:'亲，确定要抛弃我吗！',
					success:(res)=>{
						if (res.confirm) {
							this.$http({
								url:'/order/close?id='+id,
								success:res=>{
									if(res.code == 200){
										this.toggleStatus(this.currStatus);
									}
								}
							})
						}
					}
				})
			},
			goDetail(id){
				uni.navigateTo({
					url:'../order-dealing/order-dealing?id='+id
				})
			},
			setPrice(price,num){
				var totalPrice = parseFloat(price)*parseInt(num);
				return totalPrice.toFixed(4);
			},
			getStatus(id){
				var name = '',color = '';
				this.statusTabs.map(item=>{
					if(item.id == id){
						name = item.text;
						color = item.color;
					}
				})
				return {
					name,
					color
				};
			},
			getTotalNum(item){
				var total = 0;
				item.map(per=>{
					total += per.number
				})
				return total;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.order-box{
		width:750upx;
		height:calc(100vh - 268upx);
		.managebox{
			width:670upx;
			padding:60upx 0upx;
			margin:0px 40upx;
			border-bottom:2upx solid rgba(255,255,255,0.1);
			.backlog{
				width:670upx;
				height:40upx;
				display: flex;
				justify-content: space-between;
				align-items:center;
				span{
					font-size: 28upx;
					color:#fff;
				}
			}
			.goodslist{
				width:100%;
				padding-top:60upx;
				display: flex;
				flex-direction: column;
				flex-shrink:0;
				.goods{
					display: flex;
					padding-bottom:40upx;
					.image{
						width:160upx;
						height:160upx;
						margin-right: 24upx;
						overflow: hidden;
						border-radius: 8upx;
					}
				}
			}
			.account{
				display: flex;
				justify-content: flex-end;
				align-items: center;
			}
		}
	}
	.button-group{
		display: flex;
		justify-content:flex-end;
		margin-top:64upx;
		.funbtn1{
			margin-left:30upx;
		}
	}
</style>
