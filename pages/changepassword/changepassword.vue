<template>
	<view class="container">
		<uni-background />
		<uni-nav-bar 
			:opacity="scroll"
			:buttons="navButtons"
		/>
		<view class="app-container full"></view>
		<view style="margin:0upx 40upx 0;color:#fff;font-size: 48upx;">修改登录密码</view>
		<view class="textbox">
			<view class="text">
				<span class="number">手机验证码</span>
				<view>
					<input type="number" placeholder="请输入验证码" maxlength="6">
					<span>发送验证码</span>
				</view>
			</view>
			<view class="text" style="margin-top:60upx;">
				<span class="number">新登录密码</span>
				<view>
					<input type="password" maxlength="16" placeholder="请输入8~16位新登录密码">
				</view>
			</view>
			<view class="text" style="margin-top:60upx;">
				<span class="number">确认登录密码</span>
				<view>
					<input type="password" maxlength="16" placeholder="请再次输入登录密码">
				</view>
			</view>
		</view>
		<view class="commit">确认</view>
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
				}
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
	.textbox{
		width:670upx;
		margin:40upx;
		padding:40upx 40upx 60upx;
		background: #2D1F25;
		border-radius: 8upx;
		.text{
			width:590upx;
			padding-bottom:20upx;
			border-bottom:1px solid rgba(255,255,255,0.1);
			.number{
				font-size: 28upx;
				color:#fff;
			}
			view{
				margin-top:34upx;
				display:flex;
				justify-content:space-between;
				align-items:center;
				input{
					width:300upx;
					color: #FFFFFF;
					opacity: 0.5;
					font-size: 24upx;
				}
				span{	
					color: #DA53A2;
					font-size: 24upx;
				}
			}
		}
	}
	.commit{
		width:670upx;
		height:80upx;
		margin:80upx 40upx;
		background:#DA53A2;
		text-align: center;
		line-height:80upx;
		color:#fff;
		font-size: 28upx;
		border-radius: 2000upx;
	}
</style>
