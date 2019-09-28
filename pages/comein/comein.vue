<template>
	<view class="container">
		<uni-background />
		<uni-nav-bar :title="currType == 3?'转入':'转出'" textColor="#fff" :opacity="scroll" layout="center" :buttons="navButtons"></uni-nav-bar>
		<view class="app-container full">
			<view class="fun-card" style="margin:30upx 40upx;width:670upx;">
				<view class="fun-card-item">
					<view class="status-box handler-status">
						<view class="left-status">
							<text style="font-size:28upx;color:#fff;">钱包总额</text>
						</view>
						<view class="right-status">
							<text style="color:#fff;font-size: 26upx;font-family:'Montserrat-Light';margin-right:20upx;">{{walletTotal}}</text>
							<text style="font-size: 26upx;color:#999;font-family:'Montserrat-Light';">{{coin}}</text>
						</view>
					</view>
					<view class="status-box handler-status">
						<view class="left-status">
							<text style="font-size:28upx;color:#fff;">商城总额</text>
						</view>
						<view class="right-status">
							<text style="color:#fff;font-size: 26upx;font-family:'Montserrat-Light';margin-right:20upx;">{{shopTotal}}</text>
							<text style="font-size: 26upx;color:#999;font-family:'Montserrat-Light';">{{coin}}</text>
						</view>
					</view>
					<view class="form-item">
						<view class="form-label">转账金额</view>
						<view class="form-value-box">
							<view class="form-input-box">
								<input v-model="amount" class="form-input-field" :placeholder="currType == 3?'请输入转入金额':'请输入转出金额'"/>
								<view class="form-input-btns">
									<text @click="currType == 3?(amount= walletTotal):(amount = shopTotal)">{{currType == 3?'转入全部':'转出全部'}}</text>
								</view>
							</view>
						</view>
					</view>
					<view class="form-item">
						<view class="form-label" style="margin-bottom:20upx;">密码</view>
						<view class="input-field" style="padding:20upx 30upx;">
							<input type="password" maxlength="8" v-model="password" style="width:100%;font-size: 26upx;color:#c7c7c7;" placeholder="请输入钱包交易密码"/>
						</view>
					</view>
				</view>
				<view class="submit-btn" @click="submitTrans">
					{{currType == 3?'确认转入':'确认转出'}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue';
	import UniBackground from '@/components/uni-background/uni-background.vue';
	import FunButton from '@/components/fun-button.vue';
	import PasswordInputer from '@/components/possword-inputer.vue';
	export default {
		components:{
			UniNavBar,
			UniBackground,
			FunButton,
			PasswordInputer
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
					logosmall:'../../static/icons/logo_small.png',
					contacts:'../../static/icons/icon_contacts.png',
					scan:'../../static/icons/icon_scan.png'
				},
				erweima:'../../static/image.png',
				coin:'USDT',//币种
				walletTotal:0,
				shopTotal:0,
				password:'',
				currType:3,
				amount:''
			};
		},
		onLoad(option){
			this.currType = option.type;
			this.coin = option.coin;
			this.$http({
				url:'/v1/main/users/account-info',
				data:{
					type:1
				},
				success:res=>{
					if(res.code == 200){
						res.data.map(item=>{
							if(item.coin == this.coin){
								this.walletTotal = parseFloat(item.unlock_balance).toFixed(4)
							}
						})
					}
				}
			})
			this.$http({
				url:'/v1/main/users/account-info',
				data:{
					type:3
				},
				success:res=>{
					if(res.code == 200){
						res.data.map(item=>{
							if(item.coin == this.coin){
								this.shopTotal = parseFloat(item.unlock_balance).toFixed(4)
							}
						})
					}
				}
			})
		},
		onPageScroll(val){
			this.scroll = val.scrollTop;
		},
		methods:{
			getNum(num){
				return (parseFloat(num)).toFixed(2);
			},
			scanCode(){
				uni.scanCode({
					onlyFromCamera:true,
					success:res=>{
						this.toAddr = res.result;
					}
				})
			},
			//转入转出
			submitTrans(){
				if(this.password.length == 8){
					this.$http({
						url:'/v1/main/account/account-move',
						data:{
							type:parseInt(this.currType),
							coin:this.coin,
							amount:this.amount,
							pay_password:this.password
						},
						success:res=>{
							if(res.code == 200){
								uni.showToast({
									title:this.currType == 3?'转入成功':'转出成功',
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
						title:'密码长度必须为8位',
						icon:'none'
					})
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.handler-status{
		padding-bottom:30upx;
		.right-status{
			text{
				display: inline-block;
			}
		}
	}
	.user-info{
		width:750upx;
		display:flex;
		justify-content:center;
		align-items:center;
		padding:20upx 0;
		image{
			width:48upx;
			height:48upx;
			margin-right:20upx;
		}
		text{
			color:#fff;
			font-size: 32upx;
		}
	}
	.user-count{
		width:750upx;
		text-align:center;
		padding:30upx 0;
		text{
			color:#DA53A2;
			font-size: 52upx;
			font-family:'Montserrat-Bold';
		}
	}
	.erweima{
		width:200upx;
		height:200upx;
		background: #fff;
		border-radius: 12upx;
		margin:auto;
		padding:20upx;
		image{
			width:100%;
			height:100%;
		}
	}
</style>

