<template>
	<view class="password-box">
		<text class="item-text" :style="{width:size,height:size,lineHeight:size,fontSize:fontSize,background:background,borderBottom:'2upx solid '+borderColor}">{{password[0]}}</text>
		<text class="item-text" :style="{width:size,height:size,lineHeight:size,fontSize:fontSize,background:background,borderBottom:'2upx solid '+borderColor}">{{password[1]}}</text>
		<text class="item-text" :style="{width:size,height:size,lineHeight:size,fontSize:fontSize,background:background,borderBottom:'2upx solid '+borderColor}">{{password[2]}}</text>
		<text class="item-text" :style="{width:size,height:size,lineHeight:size,fontSize:fontSize,background:background,borderBottom:'2upx solid '+borderColor}">{{password[3]}}</text>
		<text class="item-text" :style="{width:size,height:size,lineHeight:size,fontSize:fontSize,background:background,borderBottom:'2upx solid '+borderColor}">{{password[4]}}</text>
		<text class="item-text" :style="{width:size,height:size,lineHeight:size,fontSize:fontSize,background:background,borderBottom:'2upx solid '+borderColor}">{{password[5]}}</text>
		<text class="item-text" :style="{width:size,height:size,lineHeight:size,fontSize:fontSize,background:background,borderBottom:'2upx solid '+borderColor}">{{password[6]}}</text>
		<text class="item-text" :style="{width:size,height:size,lineHeight:size,fontSize:fontSize,background:background,borderBottom:'2upx solid '+borderColor}">{{password[7]}}</text>
		<input type="number" class="password-input" maxlength="8" @input="changeValue"/>
	</view>
</template>

<script>
	export default {
		props:{
			size:{
				type:String,
				default:'54upx'
			},
			fontSize:{
				type:String,
				default:'32upx'
			},
			background:{
				type:String,
				default:'#F2F8FF'
			},
			borderColor:{
				type:String,
				default:'#aaa'
			}
		},
		data() {
			return {
				password:'',
				setFocus:true,
				timer:null
			};
		},
		mounted(){
		},
		methods:{
			changeValue(e){
				var len = e.detail.value.length;
				if(len>this.password.length){
					this.password = '';
					for(var i=0;i<len-1;i++){
						this.password += '•';
					}
					this.password += e.detail.value[len-1];
					clearTimeout(this.timer);
					this.timer = setTimeout(()=>{
						this.password = '';
						for(var i=0;i<len;i++){
							this.password += '•';
						}
					},1000)
				}else{
					this.password = this.password.substring(0,len);
				}
				this.$emit('input',e.detail.value);
			}
		}
	}
</script>

<style lang="scss" scoped>
	.password-box{
		width:100%;
		display:flex;
		justify-content:space-between;
		align-items:center;
		position:relative;
		.password-input{
			position: absolute;
			top:0px;
			left:0px;
			width:100%;
			height:100%;
			opacity: 0;
		}
		.item-text{
			display: block;
			width:54upx;
			height:54upx;
			background: transparent !important;
			border-bottom:2upx solid #aaa;
			line-height: 54upx;
			text-align: center;
			font-size: 32upx;
			color:#DA53A2;
			font-family:'Montserrat-Bold';
		}
	}
</style>
