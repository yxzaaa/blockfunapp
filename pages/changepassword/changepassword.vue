<template>
	<view class="container">
		<uni-background />
		<uni-nav-bar 
			:opacity="scroll"
			:buttons="navButtons"
		/>
		<view class="app-container full"></view>
		<view style="margin:0upx 40upx 0;color:#fff;font-size: 48upx;">重置登录密码</view>
		<view class="textbox">
			<view class="text">
				<span class="number">手机验证码</span>
				<view>
					<input v-model="checkCode" type="number" placeholder="请输入验证码" maxlength="6">
					<span @click="getCode">{{codeDelay === 0?'获取验证码':codeDelay+' s'}}</span>
				</view>
			</view>
			<view class="text" style="margin-top:60upx;">
				<span class="number">新登录密码</span>
				<view>
					<input v-model="password" type="password" maxlength="16" placeholder="请输入8~16位新登录密码">
				</view>
			</view>
			<view class="text" style="margin-top:60upx;">
				<span class="number">确认登录密码</span>
				<view>
					<input v-model="confirmPassword" type="password" maxlength="16" placeholder="请再次输入登录密码">
				</view>
			</view>
		</view>
		<view class="commit" @click="confirmChange">确认</view>
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
				phone:'',
				codeDelay:0,
				password:'',
				confirmPassword:'',
				checkCode:'',
				codeTimer:null,
			}
		},
		onLoad(){
			//获取用户手机号
			uni.getStorage({
				key:'userInfo',
				success:res=>{
					this.phone = res.data.login_name
				}
			})
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		methods: {
			getCode(){
				if(this.codeDelay === 0){
					this.$http({
						url:'/v1/users/login/forget-login-password/send-code?login_name='+this.phone,
						success:res=>{
							if(res.code == 200){
								this.codeDelay = 60;
								this.codeTimer = setInterval(()=>{
									if(this.codeDelay>0){
										this.codeDelay --;
									}else{
										clearInterval(this.codeTimer);
										this.codeTimer = null;
									}
								},1000);
							}else{
								uni.showToast({
									title:res.message,
									icon:'none'
								})
							}
						}
					})
				}
			},
			confirmChange(){
				if(this.password.length >= 8 && this.password === this.confirmPassword){
					this.$http({
						url:'/v1/users/login/forget-login-password/reset',
						data:{
							login_name:this.phone,
							validate_code:this.checkCode,
							new_password:this.password,
							new_password_hash:this.$md5(this.password)
						},
						success:res=>{
							if(res.code == 200){
								uni.showToast({
									title:'登录密码修改成功！',
									icon:'none'
								})
								setTimeout(()=>{
									uni.navigateBack({
										delta:1
									})
								},1500)
							}else{
								uni.showToast({
									title:res.message,
									icon:'none'
								})
							}
						}
					})
				}else{
					uni.showToast({
						title:'请检查密码输入',
						icon:'none'
					})
				}
			}
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
