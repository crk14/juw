<template>
	<div class="my" style="background:linear-gradient(to right,rgb(129,82,249),rgb(85,141,252));font-size: 14px;min-height: 760px;">
		<!-- <p class="toptitle">我的</p> -->
		<div class="tophader" style="		background-color: transparent;">
		  <van-icon name="arrow-left"  color="#ffffff" onclick="window.history.go(-1)"/>
		  <p style="color: #FFFFFF;font-weight: 440;">个人中心</p>
		</div>
		<!-- <div class="my-header">
			<img src="../assets/911.png" style="width: 100%;height: 3.28rem;" />
			<img @click="$router.push('personmsg')" v-if="infouser.avatar" :src="infouser.avatar" alt style="width: 59px;height: 59px;border-radius: 50%;
		position: absolute;left: 16px;top: 38px;" />
			<div style="position: absolute;left: 100px;top: 45px;color: #FFFFFF;font-size: 18px;">{{infouser.nick_name}}</div>
			<div style="position: absolute;left: 100px;top: 75px;color: #FFFFFF;font-size: 16px;">UID:{{infouser.id}}</div>
			<img @click="fn1(infouser.id)" style="position: absolute;left: 222px;top: 78px;width: 15px;height: 15px;" src="../assets/0796.png" />
			<div style="position: absolute;left: 100px;top: 102px;background-color: rgb(241,217,31);padding: 0 8px; font-size: 12px;line-height: 17px;text-align: center;border-radius: 10px;">{{infouser.is_need == 2?infouser.level_name:'注册会员'}}</div>
		</div> -->
		<div class="my-b">
			<img @click="$router.push('personmsg')"  :src="infouser.avatar" alt 
			 />
			<div style="font-size: 21px;">{{infouser.nick_name}}</div>
			<div style="font-size: 19px;">UID:{{infouser.id}}</div>
			<div class="box">{{infouser.is_need == 2?infouser.level_name:'注册会员'}}</div>
			<div class="div">
				<div style="border-right: 1px solid #ccc;">
					<p class="p">USDT余额</p>
					<p>{{infoamout.number}}</p>
					<p @click="$router.push('metationpage')" class="p1">充值</p>
				</div>
				<div style="border-right: 1px solid #ccc;">
					<p class="p">点卡余额</p>
					<p>{{infoamout.point_num}}</p>
					<p @click="$router.push('flash')" class="p1">购买</p>
				</div>
				<div>
					<p class="p">JUW余额</p>
					<p>{{infoamout.safe_num}}</p>
					<p @click="$router.push('carrymoney')" class="p1">提现</p>
				</div>
			</div>
			
		</div>
		<div  style="background-color: #fff;margin: 0% 5%;border-radius: 6px;">
			<div class="tablecon" style="margin-top: 13px;">
				<router-link to="/certification">
					<div >
						<p>
							<img src="../assets/3001.png" alt style="" />
							<span>实名认证</span>
						</p>
						<van-icon name="arrow" class="jtou"  size="18" color="rgb(186,186,186)"/>
					</div>
				</router-link>
				<router-link to="fenxiang">
					<div>
						<p>
							<img src="../assets/3002.png" alt style="" />
							<span>分享邀请</span>
						</p>
						<van-icon name="arrow" class="jtou" size="18" color="rgb(186,186,186)"/>
					</div>
				</router-link>
				<!-- </router-link> -->
				<router-link to="/fuli">
					<div>
						<p>
							<img src="../assets/3003.png" alt style="" />
							<span>福利中心</span>
						</p>
						<van-icon name="arrow" class="jtou" size="18" color="rgb(186,186,186)"/>
					</div>
				</router-link>
				<router-link to="/metationadr">
					<div>
						<p>
							<img src="../assets/3004.png" alt style="" />
							<span>绑定钱包</span>
						</p>
						<van-icon name="arrow" class="jtou" size="18" color="rgb(186,186,186)"/>
					</div>
				</router-link>
				<router-link to="/chagepass">
					<div>
						<p>
							<img src="../assets/3005.png" alt style="" />
							<span>安全中心</span>
						</p>
						<van-icon name="arrow" class="jtou" size="18" color="rgb(186,186,186)"/>
					</div>
				</router-link>
				<router-link to="/callcenter">
					<div>
						<p>
							<img src="../assets/3006.png" alt style="" />
							<span>联系客服</span>
						</p>
						<van-icon name="arrow" class="jtou" size="18" color="rgb(186,186,186)" />
					</div>
				</router-link>
			</div>
			<div style="height: 40px;"></div>
			<textarea cols="20" rows="10" id="biao2" style="opacity: 0;height: 1px;">{{infouser.id}}</textarea>
			<!-- <button type="button" class="changebton"  style="background: #EBEDF0;" @click="send">退出登录</button> -->
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				name: "",
				img: "",
				myimg: false,
				info: "",
				infouser: {},
				infoamout: {},
				infourl: "",
				imgpng: "",
				isshow: true,
				dayachieve: 0,
				string: 'padding: 0.02rem .3rem .32rem .3rem;',
			};
		},
		created() {
			let time = localStorage.getItem('time')
			clearInterval(time)
			this.$axios.post("/index/member/getUserInfo").then(res => {
				if (res.data.code == 0) {
					this.infouser = res.data.info;
				}
			});
			this.$axios.post("/index/mywallet/mywalletInfo").then(res => {
				if (res.data.code == 0) {
					if (res.data.day_achieve) {
						this.dayachieve = res.data.day_achieve
					}
					this.infoamout = res.data.info;
				}
			});


			var windowHeight = document.documentElement.clientHeight || document.body.clientHeight;
			if (windowHeight && windowHeight < 600) {
				this.string = 'padding: 0.32rem 0.3rem;'
			}
		},
		methods: {
			stayopen() {
				this.$toast.fail({
					message: "等待开放",
					duration: 1200
				});
			},
			fn3(){
				this.$toast.fail({ message: '钱包维护中,请联系客服', duration: 1200 });
			},
			copyUrl3(s) {
				var Url2 = document.getElementById("biao2");
				Url2.select(); // 选择对象
				document.execCommand("Copy"); // 执行浏览器复制命令
				this.$toast.success({
					message: "复制成功",
					duration: 1200
				});
			},
			fn1(s) {

				if (!!navigator.userAgent.match(/citicbankmobile/i)) {
					this.copy(s)
				} else {
					this.copyUrl3(s)
				}


			},
			copy(s) {
				var clipboard = new this.$Clipboard(".copy", {
					text: () => {
						return s;
					}
				});
				clipboard.on("success", () => {
					this.$toast.success({
						message: "复制成功",
						duration: 1200
					});
					// 释放内存
					clipboard.destroy();
				});
				clipboard.on("error", () => {
					// 不支持复制
					this.$toast.fail({
						message: "该浏览器不支持自动复制",
						duration: 1200
					});
					// 释放内存
					clipboard.destroy();
				});
			},
			savecode() {
				var ts = this;
				jsBridge.ready(function() {
					jsBridge.saveImageToAlbum(
						ts.$refs.imgsave.childNodes[1].toDataURL("image/png"),
						function(succ) {
							succ ? ts.$toast.success({
								message: "保存成功",
								duration: 1200
							}) : ts.$toast.fail({
								message: "保存失败：转码失败或没有相册使用权限",
								duration: 1200
							});
						}
					);
				});


			},
			savecode1() {
				var ts = this;
				jsBridge.ready(function() {
					jsBridge.saveImageToAlbum(ts.imgpng, function(succ) {
						succ ? ts.$toast.success({
							message: "保存成功",
							duration: 1200
						}) : ts.$toast.fail({
							message: "保存失败：转码失败或没有相册使用权限",
							duration: 1200
						});
					});
				});

			},
			send() {
				this.$toast.loading({
					message: "正在退出",
					duration: 0,
					loadingType: "spinner "
				});
				this.$axios
					.post("/index/member/logout")
					.then(res => {
						this.$toast.clear();
						this.$toast.success({
							message: res.data.msg,
							duration: 1200
						});
						setTimeout(() => {
							localStorage.clear();
							this.$router.push("login");
						}, 1200);
					})
					.catch(res => {
						this.$toast.loading({
							message: "正在强制退出",
							duration: 0,
							loadingType: "spinner "
						});
						setTimeout(() => {
							localStorage.clear();
							this.$router.push("login");
						}, 1000);
					});
			},
		},
		watch: {},
		beforeDestroy() {}
	};
</script>

<style lang="less" scoped>
	.toptitle {
		color: #333333;
		font-weight: 550;
		text-align: center;
		// padding: 0.3rem;
		height: 42px;
		line-height: 42px;
		font-size: 17px;
		background: #fff;
	}

	.header {
		display: flex;
		justify-content: space-between;
		padding: 5% 8%;
		// background: url("../assets/mytops.png") no-repeat;
		// background-size: 100%;
		background: #fff;

		// height: 2.7rem;
		.left,
		.right,
		.topr {
			text-align: center;
			font-size: 0.3rem;
			color: #333333;
		}

		.left,
		.right {
			padding-top: 0.8rem;
		}

		.numtext {
			color: #3f70cc;
			font-size: 0.3rem;
			margin: 0.1rem 0;
		}

		button {
			border: 1px solid #999999;
			border-radius: 30px;
			padding: 0 0.2rem;
			background: none;
			font-size: 0.2rem;
		}

		.topl {

			// margin-left: 0.2rem;
			p:nth-of-type(1) {
				font-size: 0.48rem;
				color: #333333;
			}

			.santext {
				height: 0.3rem;
				// background: url("../assets/jsucard.png") no-repeat;
				// background-size: 100% 100%;
				text-align: center;
				font-size: 0.24rem;
				color: #fff;
				position: relative;
				line-height: 0.3rem;
				margin-top: 0.15rem;
				min-width: 1.42rem;
				display: inline-block;

				img {
					width: 70%;
				}
			}

			p {
				font-size: 0.3rem;
				color: #999999;
				margin-top: 0.08rem;
			}
		}

		.topr {
			position: relative;

			// padding: 0.6rem 0.3rem;
			i {
				position: absolute;
				right: 0;
				top: 0.5rem;
				color: #333333;
			}

			input {
				width: 100%;
				height: 100%;
				position: absolute;
				left: 0;
				top: 0;
				opacity: 0;
			}
		}

		span {
			display: block;
			width: 1.35rem;
			height: 1.35rem;
			border-radius: 50%;
			line-height: 1.6rem;
			border: 1px solid #ccc;
			padding: 0.05rem;
			position: relative;
			left: 0.04rem;
			display: inline-block;

			.userphoto {
				width: 100%;
				height: 100%;
				border-radius: 50%;
			}
		}
	}

	.myinvicode {
		// width:88%;
		// height: 2.5rem;
		background: linear-gradient(90deg,
			rgba(33, 63, 145, 1) 0%,
			rgba(44, 76, 163, 1) 100%);
		border-radius: 6px;
		margin: 0.4rem 0.3rem;
		display: flex;
		// box-shadow: 0 0 10px #78aef9;
		padding: 0.3rem 0 0.3rem 0.2rem;

		#qrcode {
			width: 1.72rem;
			height: 1.72rem;
			border-radius: 3px;
			padding: 0.05rem;
			background: white;
			// margin: 0.34rem 0.28rem;
			margin-right: 0.2rem;
			position: relative;
			top: .1rem;

			img {
				height: 100% !important;
			}
		}

		.right {
			// margin-top: 0.15rem;
			color: white;

			p:nth-of-type(1) {
				font-size: 0.3rem;
				margin-top: -0.15rem;

				.codes {
					font-size: 0.46rem;
					position: relative;
					top: 0.05rem;
				}
			}
		}

		button {
			background: white;
			height: 0.48rem;
			color: #3f70cc;
			margin-right: 0.2rem;
			border-radius: 30px;
			font-size: 0.24rem;
			margin-top: 0.25rem;
			min-width: 1.96rem;
		}

		.bton_p {
			margin-top: -0.15rem;
		}
	}

	.tablecon {

		div {
			justify-content: space-between;
			display: flex;
			padding: 0.28rem 0.3rem;
			border-bottom: 1px solid rgb(239,239,239);
		}

		span {
			font-size: 0.32rem;
			color: rgb(168,168,168);
			margin-left: 0.3rem;
			font-family: PingFang SC;
		}

		.jtou {
			font-size: 0.44rem;
			// margin-top: 0.05rem;
			color: rgb(153, 153, 153);
		}

		img {
			position: relative;
			width: 0.52rem;
			margin-bottom: -6.5px;
		}
	}

	.child-view {
		padding-bottom: 0;
	}

	.header span {
		line-height: 1.67rem;
	}

	#qrcode canvas {
		height: 100% !important;
	}

	.topl {
		p {
			margin-left: 0.08rem;
		}
	}

	.tablecon {
		margin-top: 0.3rem;
	}

	.changebton {
		margin: 0.2rem auto;
		width: 92%;
	}

	.header .cenlevel {
		display: block;
		height: .4rem;
		line-height: .4rem;
		border-radius: 13px;
		border-color: #fff;
		background: #e70000;
		width: 2rem;
		color: #fff;
		top: .1rem;
	}

	.my-monery {
		// height: 2rem;
		background-image: linear-gradient(rgb(96, 141, 249), rgb(48, 101, 239), rgb(49, 87, 246));
		position: absolute;
		width: 91%;
		top: 157px;
		background-color: wheat;
		border-radius: 20px 20px 0 0;
		padding: 6% 5%;
		display: flex;
		justify-content: space-between;

		div {
			color: #FFFFFF;
			font-size: 14px;

			p {
				margin-top: 8px;
				font-size: 20px;
			}
		}
	}

	.my-b {
		// position: absolute;
		// top: 137px;
		width: 90%;
		// left: 5%;
		margin: 60px auto 0;
		border-radius: 6px;
		z-index: 100;
		padding: 35px 0 16px;
		background-color: #fff;
		text-align: center;
		position: relative;
		// box-shadow: 1px 4px 5px #ccc;
		.div{
			display: flex;
		}
		img{
			width: 72px;
			    height: 72px;
			    border-radius: 50%;
			    position: absolute;
			    top: -42px;
			    border: 3px solid rgb(114, 104, 255);
			    left: 39%;
		}
		.box{
			    width: 18%;
			    color: rgb(255, 255, 255);
			    font-size: 12px;
			    background: linear-gradient(to right, rgb(129, 82, 249), rgb(85, 141, 252));
			    // font-weight: 550;
			    line-height: 18px;
			    border-radius: 10px;
			    margin: 6px auto 12px;
		}
		div {
			flex: 1;
			text-align: center;
			p{
				color: rgb(154,154,154);
				font-weight: 550;
			}
			.p {
				// margin-top: 6px;
				color: rgb(198,198,198);
				font-weight:450;
				font-size: 14px;
			}
			.p1{
				font-size: 12px;
				background:linear-gradient(to right,rgb(129,82,249),rgb(85,141,252));
				line-height: 18px;
				border-radius: 3px;
				width: 60px;
				margin-left: 26%;
				margin-top: 8px;
				color: #fff;
			}
			p {
				line-height: 27px;
			}
		}
	}

	.pure_top {
		width: 100%;
		height: 100px;
		position: relative;
		z-index: 2;
		overflow: hidden;
		margin-top: -21px;

	}

	.pure_top::after {
		content: '';
		width: 140%;
		height: 100px;
		position: absolute;
		left: -20%;
		top: 0;
		border-radius: 50% 50% 0 0;
		background: #fff;

	}
</style>
