<template>
	<div class="sign">
	
	<div class="tophader"  style="border-bottom: 1px solid #eee;">
	  <van-icon name="arrow-left"   onclick="window.history.go(-1)"/>
	  <p style="font-weight: 440;">注册JUW量化</p>
	</div>
	
		<div class="togleulbox" style="padding-top: 6px;">
			<div class="loginul">
				<div>
					<p>账号</p>
					<div class="div">
						<van-icon name="contact" size="20px" color="rgb(180,180,180)"/>
						<input type="text" v-model="nick_name" placeholder="请输入您的用户名" />
						<van-icon name="clear" @click="nick_name=''" size="20px" color="rgb(205,205,205)"/>
					</div>
					
				</div>
				<div>
					<p>手机号</p>
					<div class="div">
						<img src="../assets/shouji.png" />
					<input type="text" v-model="mobile" placeholder="请输入您的手机号或邮箱" />
					<van-icon name="clear" @click="mobile=''" size="20px" color="rgb(180,180,180)"/>
					</div>
				</div>
				<div>
					<p>验证码</p>
					<div style="display: flex;">
						<div class="div" style="width: 60%;">
							<img src="../assets/yanzhen.png" />
						<input type="text" v-model="code" placeholder="请输入验证码" />
						</div>
						<button class="sendcode" @click="setcode" style="color: #fff;background: rgb(179,179,179);border-radius: 15px;height: 31px;width: 30%;">获取验证码</button>
					</div>
					
				</div>
				<div>
					<p>登录密码</p>
					<div class="div" >
						<img src="../assets/mima2.png" />
					<input  :type="str1" v-model="password" placeholder="请输入您的登录密码" />
					<van-icon  v-show="!isshow" name="eye"   @click="isshow=true" size="20px" color="rgb(205,205,205)"/>
					<van-icon v-show="isshow" name="closed-eye"  @click="isshow=false" size="20px" color="rgb(205,205,205)"/>
					</div>
				</div>
				<div>
				<div class="div" >
					<img src="../assets/mima2.png" />
					<input :type="str2" v-model="passwordc" placeholder="请再次输入您的登录密码" />
					<van-icon  v-show="!isshow1" name="eye"   @click="isshow1=true" size="20px" color="rgb(205,205,205)"/>
					<van-icon v-show="isshow1" name="closed-eye"  @click="isshow1=false" size="20px" color="rgb(205,205,205)"/>
				</div>
				</div>
				<div>
					<p>交易密码</p>
					<div class="div" >
						<img src="../assets/mima2.png" />
					<input  :type="str3" v-model="trad_password" placeholder="请输入您的交易密码" />
					<van-icon  v-show="!isshow2" name="eye"   @click="isshow2=true" size="20px" color="rgb(205,205,205)"/>
					<van-icon v-show="isshow2" name="closed-eye"  @click="isshow2=false" size="20px" color="rgb(205,205,205)"/>
					</div>
				</div>
				<div>
				<div class="div" >
					<img src="../assets/mima2.png" />
					<input :type="str4" v-model="trad_passwordc" placeholder="请再次输入您的交易密码" />
					<van-icon  v-show="!isshow3" name="eye"   @click="isshow3=true" size="20px" color="rgb(205,205,205)"/>
					<van-icon v-show="isshow3" name="closed-eye"  @click="isshow3=false" size="20px" color="rgb(205,205,205)"/>
				</div>
				</div>
				<div>
					<p>邀请码</p>
					<div class="div" >
						<img src="../assets/yanzhen.png" />
					<input type="text" v-model="invite_code" placeholder="请输入您的邀请码" />
				</div>
				</div>
				
				<div class="pass" style="position: absolute;">
					<label style="width: 0px;">
						<input type="checkbox" v-model="ispass" style="margin: 0;width: 20px;height: 20px;">
					</label>
					<span class="box">
						<van-icon :style="{opacity:ispass?1:0}" name="success" color="#306ce8" /></span> <span style="font-size: 12px;color: #000000;margin-top: -3px;margin-left: 3px;">我已阅读并完全同意接受<span
						 @click="bool = true" style="color:rgb(119,100,254)">《JUW量化用户协议》</span></span>
				</div>
				<button type="button" class="changebton" @click="send"  style="border-radius: 8px;">注册</button>
			</div>
		</div>
		
	</div>
</template>

<script>
	
	export default {
		data() {
			return {
				isclick: true,
				mobile: "",
				password: "",
				trad_password: "",
				trad_passwordc: "",
				code: "",
				invite_code: "",
				nick_name: "",
				passwordc: "",
				ispass: 0,
				type: '',
				bool: false,
				bool2: false,
				isshow:true,
				str1:'password',
				isshow1:true,
				str2:'password',
				isshow2:true,
				str3:'password',
				isshow3:true,
				str4:'password',
			};
		},
		created() {
			if (this.$route.query.id) {
				this.invite_code = this.$route.query.id;
				this.istogle = 1;
			}
			// if (this.getCookie("logmobile")) {
			//   this.logmobile = this.getCookie("logmobile");
			//   this.logpassword = this.getCookie("logpassword");
			//   this.ispass = true;
			// }
		},
		methods: {
			// 获取验证码
			setcode(e) {
				var ts = this;
				// if (!this.ismobile(this.mobile))
				//   return this.$toast.fail({
				//     message: "手机号码格式不正确",
				//     duration: 1200
				//   });
				let email = this.mobile;
				let reg = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
				if (reg.test(email)) {
					// alert("邮箱格式正确");
					ts.type = 2
				} else {
					ts.type = 1
				}
				if (ts.isclick) {
					ts.isclick = false;

					this.$axios
						.post('/index/Publics/send', {
							mobile: this.mobile,
							type: ts.type
						})
						.then(res => {
							if (res.data.code == 0) {
								this.$toast.success({
									message: res.data.msg,
									duration: 1200
								});
								var tiem = 60;
								e.target.innerText = tiem + "s";
								var timeover = setInterval(function() {
									tiem -= 1;
									e.target.innerText = tiem + "s";
									if (tiem <= 0) {
										e.target.innerText = "获取";
										clearInterval(timeover);
										ts.isclick = true;
									}
								}, 1000);
							} else {
								this.$toast.fail({
									message: res.data.msg,
									duration: 1200
								});
							}
						});
				}
			},
			ismobile(s) {
				var regPhone = /^1([358][0-9]|4[579]|66|7[0135678]|9[89])[0-9]{8}$/;
				var patternPhone = new RegExp(regPhone);
				if (patternPhone.test(s)) {
					return true;
				} else {
					return false;
				}
			},
			send() {

				if (
					!this.mobile ||
					!this.password ||
					!this.trad_password ||
					!this.trad_passwordc ||
					!this.code ||
					!this.nick_name ||
					!this.passwordc ||
					!this.invite_code
				) {
					this.$toast.fail({
						message: "请填写完整",
						duration: 1200
					});
					return;
				}
				// if (!this.ismobile(this.mobile))
				//   return this.$toast.fail({
				//     message: "手机号码格式不正确",
				//     duration: 1200
				//   });
				if (this.password != this.passwordc)
					return this.$toast.fail({
						message: "两次密码不一致",
						duration: 1200
					});

				if (this.trad_password != this.trad_passwordc)
					return this.$toast.fail({
						message: "两次交易密码不一致",
						duration: 1200
					});
				if (this.ispass == 0) {
					this.$toast.fail({
						message: "请阅读用户协议",
						duration: 1200
					});
					return
				}
				this.$axios
					.post("/index/Publics/doRegister", {
						mobile: this.mobile,
						password: this.password,
						trad_password: this.trad_password,
						code: this.code,
						invite_code: this.invite_code,
						nick_name: this.nick_name,
						type: this.type,
					})
					.then(res => {
						if (res.data.code == 0) {
							this.$toast.success({
								message: res.data.msg,
								duration: 1200
							});
							setTimeout(() => {
								this.$router.push("/login");
							}, 1200);
						} else {
							this.$toast.fail({
								message: res.data.msg,
								duration: 1200
							});
						}
					});
			}
			// setCookie(name, value) {
			//   var exp = new Date();
			//   exp.setTime(exp.getTime() + 7 * 24 * 60 * 60 * 1000);
			//   document.cookie =
			//     name + "=" + escape(value) + ";expires=" + exp.toGMTString();
			// },
			// getCookie(name) {
			//   var arr,
			//     reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
			//   if ((arr = document.cookie.match(reg))) return unescape(arr[2]);
			//   else return null;
			// }
		},
		watch: {
			isshow(oldvalue,newvalue) {
			  if(oldvalue){
				  this.str1 = 'password'
			  }else{
				  this.str1 = 'text'
			  }
			},
			isshow1(oldvalue,newvalue) {
			  if(oldvalue){
				  this.str2 = 'password'
			  }else{
				  this.str2 = 'text'
			  }
			},
			isshow2(oldvalue,newvalue) {
			  if(oldvalue){
				  this.str3 = 'password'
			  }else{
				  this.str3 = 'text'
			  }
			},
			isshow3(oldvalue,newvalue) {
			  if(oldvalue){
				  this.str4= 'password'
			  }else{
				  this.str4 = 'text'
			  }
			},
		}
	};
</script>

<style lang="less" scoped>
	@import url("../less/login.less");

	.headtop {
		text-align: left;
	}
	.loginul .div{
		margin: 0;
		background-color: rgb(247,247,247);
		border-radius: 10px;
		padding: 10px 10px;
		display: flex;
		input{
			margin: 0;
			font-size: 12px;
			margin-left: 12px;
			width: 80%;
		}
		img{
			height: 20px;
			width: 20px;
			margin-top: -3px;
		}
	}
	.loginul {
		div{
			margin-top: 15px;
		}
		p{
			margin-bottom: 8px;
		}
	}
	.loginul .pass {
		display: flex;
		justify-content: space-between;
		margin-top: 0.3rem;

		a {
			position: relative;
			top: -0.2rem;
		}

		label {
			width: 40%;
			position: relative;
			color: #3c8cff;
		}

		.text {
			position: relative;
			font-size: 0.26rem;
			top: -0.1rem;
			left: 0.1rem;
		}

		.box {
			border: 1px solid rgb(205,205,205);
			font-size: 0.3rem;
			height: 0.26rem;
			text-align: center;
			line-height: 0.3rem;
			width: 0.26rem;
			display: inline-block;
			border-radius: 50%;
			position: relative;
			top: -0.06rem;
			background-color: rgb(205,205,205);
		}
		input {
			position: absolute;
			width: 100%;
			height: 100%;
			z-index: 1;
			opacity: 0;
		}
	}

	.headtop {
		margin: 0.3rem 0 0.6rem 0.3rem;
	}

	.pass {
		display: flex;
		justify-content: space-between;
		margin-top: 0.3rem;

		a {
			position: relative;
			top: -0.29rem;
		}

		label {
			width: 40%;
			position: relative;
			color: rgb(48, 108, 232);
		}

		.text {
			position: relative;
			font-size: 0.29rem;
			top: -0.1rem;
			left: 0.1rem;
		}

		.box {
			border: 2px solid rgb(48, 108, 232);
			font-size: 0.3rem;
			height: 10px;
			text-align: center;
			line-height: 10px;
			width: 10px;
			display: inline-block;
			border-radius: 50%;
			position: relative;
			top: -2px;
		}

		input {
			position: absolute;
			width: 100%;
			height: 100%;
			z-index: 1;
			opacity: 0;
		}
	}

	.poup {
		h2 {
			text-align: center;
			margin: 10px 0;
			color: rgb(0, 27, 49);
			font-size: 18px;
			font-weight: 550;
		}

		h4 {
			margin: 0px 0 5px 0;
			color: rgb(0, 27, 49);
			text-align: center;
			font-size: 16px;
		}

		.p {
			padding: 0 .28rem;
			text-indent: 24px;
			color: rgb(138, 144, 151);
			font-size: 14px;
		}

		.p1 {
			text-indent: 0px;
		}

		h3 {
			margin: 4px 0;
			padding: 0 .28rem;
			font-weight: 550;
			font-size: 16px;
			color: rgb(0, 27, 49);
		}

		h5 {
			color: rgb(138, 144, 151);
			padding: 0 .28rem;
			font-size: 16px;
			margin: 0px 0 2px 0;
		}

	}
</style>
