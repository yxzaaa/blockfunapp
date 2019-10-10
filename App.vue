<script>
	/**
	 * vuex管理登陆状态，具体可以参考官方登陆模板示例
	 */
	export default {
		onLaunch: function() {
			//判断登录状态
			let userInfo = uni.getStorageSync('userInfo');
			if(!userInfo || !userInfo.uid){
				uni.reLaunch({
					url:'/pages/login/login'
				})
			}
		},
		onShow: function() {
			
		},
		onHide: function() {
		},
	}
</script>

<style lang='scss'>
	/*
		全局公共样式和字体图标
	*/
	@import url('./static/fonts/montserrat.css');
   
	@font-face {
		font-family: yticon;
		font-weight: normal;
		font-style: normal;
		src: url('https://at.alicdn.com/t/font_1078604_w4kpxh0rafi.ttf') format('truetype');
	}
	
	@font-face {
		font-family: 'PingFang';
		src: url('./static/fonts/PingFang.ttf') format('truetype');  
	}

	view,
	scroll-view,
	swiper,
	swiper-item,
	cover-view,
	cover-image,
	icon,
	text,
	rich-text,
	progress,
	button,
	checkbox,
	form,
	input,
	label,
	radio,
	slider,
	switch,
	textarea,
	navigator,
	audio,
	camera,
	image,
	video {
		box-sizing: border-box;
		word-break: break-all;
	}
	
	*{
		font-family: 'PingFang';
	}
	
	body{
		background-color:#15030B;
	}
	
	text span{
		font-family: inherit;
	}
	.image{
		background-image: url(/static/errorImage.jpg);
		background-size:100% 100%;
	}
	/* 骨架屏替代方案 */
	.Skeleton {
		background: rgba(255,255,255,0.1);
		border-radius: 8upx;
		margin-bottom:12upx;
		animation: flicker 1.6s infinite;
	}

	/* input 样式 */
	.input-placeholder {
		color: #999999;
	}

	.placeholder {
		color: #999999;
	}
	.container{
		position: relative;
	}
	.app-container{
		&.full{
			padding-top:194upx;
		}
		&.fixbutton{
			padding-bottom:120upx;
		}
		.empty-box{
			width:750upx;
			padding:80upx 120upx;
			display: flex;
			justify-content: center;
			align-items: center;
			flex-wrap: wrap;
			image{
				margin-bottom:80upx;
			}
			text{
				font-size: 24upx;
				color:rgba(255,255,255,0.5);
			}
		}
	}
	.main-title{
		font-size: 32upx;
		color:#fff;
		text-align: center;
		padding:30upx 0upx;
	}
	.section-header{
		width:750upx;
		padding:40upx;
		padding-bottom:30upx;
		display:flex;
		justify-content:flex-start;
		align-items:center;
		.section-icon{
			width:40upx;
			height:40upx;
			margin-right:10upx;
		}
		.section-title{
			font-size: 32upx;
			color:#fff;
		}
		.toggle-box{
			line-height: 48upx;
			text{
				color:rgba(255,255,255,0.5);
				font-size: 28upx;
				padding:0upx 14upx;
				&.active{
					color:#DA53A2;
					font-weight: bold;
				}
			}
		}
	}
	.section-subtitle{
		font-size: 26upx;
		color:#fff;
		line-height: 54upx;
	}
	.section-describe{
		font-size: 26upx;
		color:#c7c7c7;
		line-height: 54upx;
	}
	.section-part{
		font-size: 26upx;
		color:#999;
		line-height: 54upx;
	}
	.section-content{
		padding:5px 40upx;
	}
	.button-group{
		display:flex;
		justify-content:space-between;
		.fun-btn{
			/* margin-left:20upx; */
		}
	}
	.fun-horizen{
		width:100%;
		height:2upx;
		background: rgba(255,255,255,.1);
	}
	.fun-card{
		width:670upx;
		background: rgba(45,31,37,0.7);
		border-radius: 8upx;
		overflow:hidden;
		position:relative;
		.fun-card-item{
			padding:40upx;
		}
		.item-horizen{
			display: flex;
			justify-content: flex-start;
			align-items: center;
		}
		.fun-card-buttons{
			width:100%;
			padding:20upx 0px;
			display: flex;
			justify-content: space-between;
		}
		.submit-btn{
			width:100%;
			height:100upx;
			line-height: 100upx;
			text-align: center;
			font-size:28upx;
			color:#DA53A2;
			background: rgba(218,83,162,0.2);
		}
	}
	.button-image{
		width:32upx;
		height:32upx;
		margin:30upx;
		display: block;
	}
	.fixed-buttons{
		position: fixed;
		bottom:0px;
		left:0px;
		width:750upx;
		padding:20upx 40upx;
		background: #2F282B;
		z-index:110;
		.certs{
			width:100%;
			display: flex;
			align-items: center;
			justify-content: center;
			padding:10upx 0;
			padding-top:30upx;
			.check-cert{
				width:24upx;
				height:24upx;
				margin-right:10upx;
			}
			.cert-text{
				color:#999;
				font-size:24upx;
				.cert-name{
					color:#DA53A2;
				}
			}
		}
	}
	.fun-table{
		width:100%;
		border-radius:20upx;
		overflow:hidden;
		.fun-table-header{
			width:100%;
			line-height: 74upx;
			text-align: center;
			background: #3A1F2D;
			color:#fff;
			font-size: 28upx;
			&.trans{
				background: transparent;
			}
		}
		.fun-table-body{
			width:100%;
			.fun-table-tr{
				width:100%;
				border-bottom:1px solid #45323B;
				display:flex;
				justify-content:space-between;
				overflow:hidden;
				&.noborder{
					border:none;
				}
				.trans{
					background:transparent;
					padding:15upx;
					line-height: 44upx;
					color:#999;
					text-align: center;
					font-size:26upx;
					&.white{
						color:#fff;
					}
					&.four{
						width:25%;
					}
					&.five{
						width:20%;
					}
					&.small{
						font-size:24upx;
					}
				}
				.deep{
					background: #281920;
					line-height: 74upx;
					color:#999;
					text-align: center;
					font-size:26upx;
					&.white{
						color:#fff;
					}
					&.four{
						width:25%;
					}
					&.five{
						width:20%;
					}
					&.small{
						font-size:24upx;
					}
				}
				.light{
					background: #311C26;
					line-height: 74upx;
					color:#999;
					text-align:center;
					font-size:26upx;
					&.white{
						color:#fff;
					}
					&.four{
						width:25%;
					}
					&.five{
						width:20%;
					}
					&.small{
						font-size:24upx;
					}
				}
			}
		}
	}
	
	.status-box{
		width:100%;
		display:flex;
		justify-content: space-between;
		.left-status{
			text{
				display: block;
				line-height: 52upx;
			}
		}
		.right-status{
			text-align:right;
			text{
				display: block;
				line-height: 52upx;
			}
		}
	}
	.check-box{
		border:1px solid rgba(255,255,255,0.5);
		border-radius: 18upx;
		width:96upx;
		height:36upx;
		overflow: hidden;
		display: flex;
		align-items: center;
		margin:0px;
		text{
			transition: all .3s;
			width:56upx;
			border-radius: 0px;
			height:26upx;
			border-radius:13upx;
			background:rgba(255,255,255,0.5);
			position:relative;
			transform:translateX(35upx);
			&.active{
				background:rgba(255,255,255,1);
				transform:translateX(4upx);
			}
		}
	}
	
	.form-item{
		width:100%;
		padding-bottom:20upx;
		.form-label{
			display: block;
			width:100%;
			font-size: 28upx;
			color:#fff;
			line-height: 40upx;
			.des-item{
				float:right;
				color:#999;
				font-size: 24upx;
			}
		}
		.form-value-box{
			padding-bottom:30upx;
			view{
				display: flex;
				align-items: center;
			}
		}
		.input-field{
			width:100%;
			border-radius: 6upx;
			background: #15030B;
			line-height: 72upx;
			padding:0upx 20upx;
			display:flex;
			justify-content:space-between;
			flex-wrap:wrap;
			.input-left{
				color:#999999;
			}
			.input-right{
				color:#999999;
			}
		}
		.form-input-box{
			padding-top:40upx;
			padding-bottom:20upx;
			display: flex;
			justify-content: space-between;
			border-bottom:1px solid rgba(255,255,255,.2);
			.form-input-field{
				width:calc(100% - 150upx);
				font-size: 26upx;
				color:#c7c7c7;
			}
			.form-input-btns{
				padding:0upx 20upx;
				width:150upx;
				display:flex;
				justify-content:flex-end;
				image{
					width:40upx;
					height:40upx;
					margin-left:15upx;
				}
				text{
					color:#DA53A2;
					font-size:26upx;
				}
			}
		}
	}
	.radio-box{
		width:100%;
		height:56upx;
		display:flex;
		justify-content:space-between;
		align-items: center;
		border-radius:8upx;
		margin-top:10upx;
		border:1px solid rgba(255,255,255,.5);
		position:relative;
		&.noborder{
			border:none;
		}
		text{
			line-height:56upx;
			display: block;
			color:#fff;
			font-size:26upx;
			text-align:center;
			border-radius:8upx;
			font-weight:bolder;
			&.active{
				background:#fff;
				color:#DA53A2;
			}
		}
	}
	@keyframes show{
		from {opacity: 0;transform:scale(1.1);}
		to {opacity: 1;transform:scale(1);}
	}
	.modal-box{
		opacity: 1;
		animation: show ease 0.3s 1;
		transition: all .5s;
		position: fixed;
		top:0px;
		left:0px;
		width:750upx;
		height:100vh;
		background: rgba(0,0,0,.5);
		z-index:120;
		display:flex;
		justify-content:center;
		align-items:center;
		transform:scale(1);
		.modal{
			width:560upx;
			border-radius: 12upx;
			background: #fff;
			overflow: hidden;
			.modal-top-item{
				width:100%;
				padding:20upx 40upx;
				.modal-title{
					width:100%;
					line-height: 64upx;
					font-size: 30upx;
					color:#000;
					padding-bottom:20upx;
					text-align: center;
					font-weight: bold;
				}
				.modal-content{
					width:100%;
					line-height: 48upx;
					font-size: 26upx;
					color:#000;
					.input-box{
						width:100%;
						display:flex;
						justify-content:center;
						align-items:center;
						
						input{
							display: block;
							width:300upx;
							height:64upx;
							line-height: 64upx;
							color:#DA53A2;
							background: #F2F8FF;
							text-align: center;
							font-weight: bold;
							font-size: 36upx;
							font-family:'Montserrat-Bold';
						}
					}
				}
			}
			.modal-btns{
				border-top:1px solid #eee;
				width:100%;
				display: flex;
				justify-content: space-between;
				view{
					width:50%;
					line-height: 100upx;
					height:100upx;
					color:#000;
					text-align: center;
					font-size: 32upx;
					&:active{
						background: #eee;
					}
				}
			}
		}
	}
	.swiper-box{
		width:750upx;
		height:calc(100vh - 250upx);
	}
	.fix-tabs-box{
		width:750upx;
		padding:0upx 40upx;
		display: flex;
		justify-content: center;
		border-bottom:1px solid rgba(255,255,255,0.2);
		position:relative;
		.fix-tabs-item{
			width:50%;
			line-height: 76upx;
			height:76upx;
			display:flex;
			justify-content:center;
			text{
				color:rgba(255,255,255,0.5);
				transition:all .4s;
				font-size:32upx;
				&.active{
					color:#fff;
					font-weight: bold;
					border-bottom:3upx solid #fff;
				}
			}
		}
	}
</style>
