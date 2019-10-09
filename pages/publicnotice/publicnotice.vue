<template>
	<scroll-view class="container">
		<uni-background />
		<uni-nav-bar title="公告" textColor="#fff" :opacity="scroll" layout="center" :buttons="navButtons"></uni-nav-bar>
		<view class="app-container full">
			<view class="notice-head">
				<text style="font-size: 40upx;font-family:'Montserrat-Light';">{{getDate()}}</text>
				<text>{{getDay()}}</text>
			</view>
			<view class="notice-list">
				<block v-for="(item,index) in notices" :key="item.id">
					<view class="notice-item">
						<view class="left-line">
							<image :src="imageLib.ellipse"></image>
							<view class="bottom-line" v-if="index !== notices.length-1"></view>
						</view>
						<view class="right-box">
							<text style="color:#fff;font-size: 30upx;">{{item.title}}</text>
							<text>{{getItemDate(item.created_on)}}</text>
							<text>亲爱的社区用户：</text>
							<view :class="['content-box',item.show?'':'show-more']" scroll-y>
								<text>{{item.content}}</text>
							</view>
							<view style="color:#DA53A2;font-size:28upx;margin-top:20upx;margin-bottom: 40upx;display: inline-block;" @click="showMore(index)">{{item.show?'收起':'查看详情'}}</view>
						</view>
					</view>
				</block>
			</view>
		</view>
	</scroll-view>
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
				imageLib:{
					ellipse:'../../static/icons/Ellipse.png'
				},
				notices:[]
			};
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		onLoad(){
			//获取公告列表
			this.$http({
				url:'/announce/search',
				success:res=>{
					console.log(res);
					if(res.code == 200){
						this.notices = res.data;
						this.notices.map(item=>{
							item.show = false;
						})
					}
				}
			})
		},
		methods:{
			showMore(index){
				var isShow = this.notices[index].show?false:true;
				var item = this.notices[index];
				item.show = isShow;
				this.$set(this.notices,index,item);
				if(isShow){
					this.notices.map((val,i)=>{
						if(i !== index){
							val.show = false;
							this.$set(this.notices,i,val);
						}
					})
				}
			},
			getItemDate(timestamp){
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
			getDate(){
				var date = new Date();
				var month = (date.getMonth()+1)>=10?date.getMonth()+1:'0'+(date.getMonth()+1);
				var day = date.getDate()>=10?date.getDate():'0'+date.getDate();
				return date.getFullYear() + '/' + month + '/' + day;
			},
			getDay(){
				var day = new Date().getDay();
				var weekday=["星期日","星期一","星期二","星期三","星期四","星期五","星期六"];
				return weekday[day];
			}
		}
	}
</script>

<style lang="scss" scoped>
	.notice-head{
		width:670upx;
		padding:40upx 0upx;
		padding-top:10px;
		margin:0upx 40upx;
		display:flex;
		justify-content:space-between;
		border-bottom:1px solid rgba(255,255,255,.1);
		text{
			font-size:32upx;
			color:#fff;
		}
	}
	.notice-list{
		width:750upx;
		padding:60upx 40upx;
		.notice-item{
			width:100%;
			height:auto;
			display: flex;
			justify-content: flex-start;
			.left-line{
				width:26upx;
				// height:100%;
				min-height:340upx;
				image{
					width:26upx;
					height:26upx;
					display: block;
				}
				.bottom-line{
					width:5upx;
					background: rgba(255,255,255,0.2);
					height:calc(100% - 26upx);
					margin:auto;
				}
			}
			.right-box{
				width:calc(100% - 26upx);
				padding:0upx 20upx;
				position:relative;
				top:-12upx;
				text{
					font-size: 24upx;
					line-height: 48upx;
					color:#999;
					display: block;
				}
				.content-box{
					width:100%;
					transition:all .3s;
					overflow:hidden;
					&.show-more{
						max-height: 190upx;
					}
				}
			}
		}
	}
</style>
