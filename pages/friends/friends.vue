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
					<view style="font-size: 36upx;font-weight: bold;color:rgba(255,255,255,1);text-align: center;padding-top: 20upx;">{{friendCount}}</view>
				</view>
			</view>
			<view v-if="friends" style="font-size: 26upx;font-weight: bold;width:750upx;color:rgba(255,255,255,0.5);text-align: center;padding-top:60upx;padding-bottom:30upx;border-bottom:2upx solid rgba(255,255,255,0.1);">好友明细</view>
			<view v-if="friends" class="fortunetype">
				<view class="typeinfo">
					<view>
						<span class="type" style="width:30%">好友ID</span>
						<span class="type" style="width:30%">好友手机号</span>
						<span class="type" style="width:40%">好友注册日期</span>
					</view>
					<view v-for="(item,index) in friends" :key="index">
						<span class="numb" style="width:30%">{{item.uid}}</span>
						<span class="numb" style="width:30%">{{item.login_name}}</span>
						<span class="numb" style="width:40%">{{getDate(item.time)}}</span>
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
				friendCount:0,
				friends:[]
			}
		},
		onLoad(){
			//获取我的好友列表
			this.$http({
				url:'/v1/main/users/my-friends',
				success:res=>{
					if(res.code == 200){
						this.friendCount = res.data.count;
						this.friends = res.data.friends;
					}
				}
			})
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		methods: {
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
