<template>
	<div style="height: 100%;background:linear-gradient(to right,rgb(129,82,249),rgb(85,141,252));font-size: 14px;min-height: 760px;">
		<div class="tophader"  >
		  <van-icon name="arrow-left"   onclick="window.history.go(-1)"/>
		  <p style="font-weight: 440;">分享邀请</p>
		</div>
		<!-- <img style="width: 100%;height: 100%;position: relative;" src="../assets/baana6.png"/> -->
		<div class="body">
			<div id="qrcode" ref="imgsave">
				<img v-show="myimg" :src="img" alt />
			</div>
			<p>我的邀请码: <span>{{info}}</span> </p>
			<button @click="fn1(info,true)">立即使用</button>
		</div>
		<div class="body" style="margin: -28px auto 0;padding-top: 15px;height: 18%;">
			<p style="margin-top: 0px;"><i>一</i>  第三方分享 <i>一</i></p>
			<div class="div">
				<div>
					<img src="../assets/weix1.png" @click="fn2(infourl)" />
					<p>微信分享</p>
				</div>
				<div>
					<img src="../assets/qq2.png"  @click="fn2(infourl)"/>
					<p>QQ分享</p>
				</div>
				<div @click="savecode1">
					<img src="../assets/haibao.png" />
					<p>生成海报</p>
				</div>
			</div>
		</div>
		<!-- <div class="button">
			<div  @click="fn1(infourl,false)" >复制邀请链接</div>
			<div type="button" @click="savecode1" >生成邀请海报</div>
		</div> -->
	
		<textarea cols="20" rows="10" id="biao1" style="opacity: 0;height: 1px;">{{info}}</textarea>
		<textarea cols="20" rows="10" id="biao2" style="opacity: 0;height: 1px;">{{infourl}}</textarea>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				img: "",
				myimg: false,
				info: "",
				infourl: '',
				imgpng: '',
				number: 0,
				memberaccount: 0,
				list: [],
				ishow: false
			};
		},
		created() {
			console.log(navigator.userAgent)
			this.$axios.post("/index/member/getUserInfo").then(res => {
				if (res.data.code == 0) {
					if (res.data.info.is_need == 2) {
						this.ishow = true
					}
				}
			});
			this.$axios.post("/index/member/getInviteImg").then(res => {
				if (res) {
					if (res.data.code == 0) {
						this.imgpng = location.origin + res.data.info;
					}
				}
			});
			this.$axios.get("/index/welfarecenter/member_account").then(res => {
				if (res.data.code == 0) {
					this.memberaccount = res.data.member_account
				}
			});
			this.$axios.get("/index/rank/get_share_rank").then(res => {
				if (res.data.code == 0) {
					this.list = res.data.data
					// this.memberaccount = res.data.member_account
				}
			});
		},
		mounted() {
			this.$axios.post("/index/member/spread").then(res => {
				if (res.data.code == 0) {
					// this.name = res.data.info.nick_name;
					this.number = res.data.data.invite_num
					this.info = res.data.info;
					new this.$QRCode("qrcode", {
						width: 85,
						height: 85,
						text: `http://${res.data.share_url}` + "/dist/#/sign?id=" + res.data.info
					});

					this.infourl = `http://${res.data.share_url}` + "/dist/#/sign?id=" + res.data.info;
					// this.img = this.$refs.imgsave.childNodes[1].toDataURL("image/png");
					// this.myimg = false;
				}

			});
		},
		methods: {
			copyUrl2() {
				var Url2 = document.getElementById("biao1");
				Url2.select(); // 选择对象
				document.execCommand("Copy"); // 执行浏览器复制命令
				this.$toast.success({
					message: "复制成功",
					duration: 1200
				});
			},
			copyUrl3() {
				var Url2 = document.getElementById("biao2");
				Url2.select(); // 选择对象
				document.execCommand("Copy"); // 执行浏览器复制命令
				this.$toast.success({
					message: "复制成功",
					duration: 1200
				});
			},
			fn2(s) {
				console.log(s)
				var clipboard = new this.$Clipboard(".copy", {
					text: () => {
						return s;
					}
				});
				clipboard.on("success", () => {
					this.$toast.success({
						message: "复制成功,请打开微信或QQ分享",
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
			savecode1() {
				var ts = this;
					// if(jsBridge.ready()){
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
					// }else{
					// 	if(ts.imgpng){
					// 			window.open(ts.imgpng)
					// 		}
					// }
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
		},

	};
</script>

<style lang="less" scoped>
	.button{
		position: absolute;
		width: 70%;
		bottom: 8%;
		display: flex;
		justify-content: space-between;
		left: 15%;
		div{
			width: 45%;
			border: 1px solid #FFFFFF;
			line-height: 30px;
			color: #FFFFFF;
			text-align: center;
			border-radius: 3px;
		}
	}
	.body{
		width: 88%;
		margin: 50px auto;
		height: 44%;
		background-color: #FFFFFF;
		border-radius: 6px;
		text-align: center;
		padding-top: 30px;
		.div{
			display: flex;
			justify-content: space-around;
			img{
				height: 65px;
				width: 65px;
				margin-top: 13px;
			}
			p{
				margin-top: 1px;
			}
		}
		p{
			font-size: 14px;
			margin-top: 18px;
			span{
				font-size: 22px;
				font-weight: 550;
				color: rgb(42,103,254)
			}
			i{
				margin: 0 4px;
			}
		}
		button{
			width: 86%;
			line-height: 42px;
			border-radius: 6px;
			font-size: 15px;
			color: #ffffff;
			margin: 28px 0;
			background-color: rgb(42,103,254);
		}
		#qrcode{
			width: 180px;
			height: 180px;
			margin: 0 auto;
			margin-top: 13px;
		}
	}
</style>
