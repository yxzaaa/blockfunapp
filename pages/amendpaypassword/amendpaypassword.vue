<template>
	<view class="container">
		<uni-background />
		<uni-nav-bar 
			:opacity="scroll"
			:buttons="navButtons"
		/>
		<view class="app-container full" style="padding-left:40upx;padding-right:40upx;">
			<view style="width:670upx;line-height:68upx;color:#fff;font-size: 48upx;">
				{{stepLib[currStep].title}}
			</view>
			<view style="line-height:40upx;color:#fff;font-size: 28upx;padding-top:30upx;padding-bottom:60upx;">{{stepLib[currStep].content}}</view>
			<view style="width:670upx;">
				<possword-inputer @input="setPassword" v-if="currStep == 0" size="74upx" borderColor="rgba(255,255,255,0.4)" fontSize="48upx"></possword-inputer>
			</view>
			<view style="width:670upx;">
				<possword-inputer @input="setPassword" v-if="currStep == 1" size="74upx" borderColor="rgba(255,255,255,0.4)" fontSize="48upx"></possword-inputer>
			</view>
			<view style="width:670upx;">
				<possword-inputer @input="setPassword" v-if="currStep == 2" size="74upx" borderColor="rgba(255,255,255,0.4)" fontSize="48upx"></possword-inputer>
			</view>
			<navigator style="width:150upx;color:#DA53A2;font-size: 28upx;padding-top:30upx;padding-bottom:80upx;" url="../forgetpaypassword/forgetpaypassword">忘记密码？</navigator>
			<fun-button value="确认" block large width="670upx" @handle="confirmStep"></fun-button>
		</view>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	import FunButton from '@/components/fun-button.vue';
	import PosswordInputer from '@/components/possword-inputer.vue';
	export default {
		components:{
			UniNavBar,
			UniBackground,
			FunButton,
			PosswordInputer
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
				stepLib:[
					{
						title:'修改支付密码',
						content:'请输入原支付密码'
					},
					{
						title:'输入新支付密码',
						content:'请输入新支付密码'
					},
					{
						title:'确认新支付密码',
						content:'请再次输入新支付密码'
					},
				],
				currStep:0,
				oldPassword:'',
				newPassWord:'',
				confirmPassword:''
			}
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		methods: {
			setPassword(val){
				if(this.currStep == 0){
					this.oldPassword = val;
				}else if(this.currStep == 1){
					this.newPassWord = val;
				}else if(this.currStep == 2){
					this.confirmPassword = val;
				}
			},
			confirmStep(){
				if(this.currStep == 0 && this.oldPassword.length == 8){
					//验证原密码
					this.$http({
						url:'/v1/main/users/reset-confirm-account-payment-password-by-password',
						data:{
							password:this.oldPassword,
							pay_password:this.oldPassword,
							pay_password_hash:this.$md5(this.oldPassword)
						},
						success:res=>{
							if(res.code == 200){
								this.currStep = 1;
							}else{
								uni.showToast({
									title:res.message,
									icon:'none'
								})
							}
						}
					})
				}else if(this.currStep == 1 && this.newPassWord.length == 8){
					this.currStep = 2;
				}else if(this.currStep == 2){
					//修改支付密码
					if(this.newPassWord == this.confirmPassword){
						this.$http({
							url:'/v1/main/users/reset-confirm-account-payment-password-by-password',
							data:{
								password:this.oldPassword,
								pay_password:this.newPassWord,
								pay_password_hash:this.$md5(this.newPassWord)
							},
							success:res=>{
								if(res.code == 200){
									uni.showToast({
										title:'密码修改成功',
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
									this.currStep = 0;
								}
							}
						})
					}else if(this.confirmPassword.length == 8){
						uni.showToast({
							title:'两次输入的密码不一致',
							icon:'none'
						});
						this.currStep = 0;
					}
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.input{
		width:750upx;
		height:94upx;
		padding:0 46upx 0 40upx; 
		margin:40upx 0;
		display:flex;
		justify-content:space-between;
		view{
			width:94upx;
			height:94upx;
			background:#fff;
			opacity: 0.1;
			border-radius: 4upx;
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
