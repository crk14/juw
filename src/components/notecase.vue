<template>
	<div class="note">
		<div class="tophader" style="background-color: #fff;margin-bottom: 8px;">
			<van-icon name="arrow-left" onclick="window.history.go(-1)" />
			<p>JUW量化系统</p>
		</div>
		<van-tabs v-model="active"  color="#fff" background="rgb(204,204,204)" type="card" style="margin: 0;">
			<van-tab title="API管理" :title-style="active==0?'color:#000':'color:#fff'">
				<p class="open">已开通自动交易</p>
				<div class="tolead" @click="$router.push({path:'global',query:{id:1}})">
					<p>
						<img src="../assets/src_resource_image_page_huobi_logo@2x.png" alt />火币全球站
					</p>
					<p>
						<img src="../assets/gou.png" alt />导入API
					</p>
				</div>
				<div class="tolead" @click="$router.push({path:'global',query:{id:4}})">
					<p>
						<img src="../assets/src_resource_icon_user_exchange_okex@2x.png" alt />OKEX
					</p>
					<p>
						<img src="../assets/gou.png" alt />导入API
					</p>
				</div>
			</van-tab>
			<van-tab title="现货量化" :title-style="active==1?'color:#000':'color:#fff'">

				<p style="line-height: 110px;text-align: center;">敬请期待</p>
			</van-tab>
			<van-tab title="合约量化" :title-style="active==2?'color:#000':'color:#fff'">
				<div style="background-color: #fff;margin: -5px 10px 0;padding-top:5px" >
					<div class="note-t" >
						<div class="div">
							 选择交易所
							 <div></div>
						</div>
						<div class="div2">
							<div  :class="{'avtive':bourse==0}" @click="bourse=0">Huobi</div>
							<div  :class="{'avtive':bourse==1}" @click="bourse=1">OKEX</div>
							<!-- <div :class="{'avtive':bourse==2}" @click="bourse=2">Coinbene</div> -->
						</div>
					</div>
					<div class="note-t">
						<div class="div">
							 选择交易对
							 <div></div>
						</div>
						<div class="div2">
							<div :class="{'avtive':symbol=='BTC'}" @click="symbol='BTC'">BTC/USDT</div>
							<div :class="{'avtive':symbol=='ETH'}" @click="symbol='ETH'">ETH/USDT</div>
						</div>
					</div>
					<div class="note-t">
						<div class="div">
							 选择策略
							 <div></div>
						</div>
						<div class="div2">
							<div :class="{'avtive':type==2}" @click="type=2">智能趋势</div>
							<div :class="{'avtive':type==3}" @click="symbol=type=3">多空网格</div>
						</div>
					</div>
					<div class="note-t">
						<div class="div" style="line-height: 85px;">
							 选 择 仓 位
							 <div style="height: 75px;"></div>
						</div>
						<div class="div2 div3">
							<div  style="margin-bottom: 5px;" :class="{'avtive':monery==5000}" @click="monery=5000">5000USDT</div>
							<div style="margin-bottom: 5px;" :class="{'avtive':monery==10000}" @click="monery=10000">10000USDT</div>
							<div style="margin-bottom: 5px;" :class="{'avtive':monery==20000}" @click="monery=20000">20000USDT</div>
							<div  style="margin-bottom: 5px;" :class="{'avtive':monery==50000}" @click="monery=50000">50000USDT</div>
							<div style="margin-bottom: 5px;":class="{'avtive':monery==100000}" @click="monery=100000">100000USDT</div>
							<div style="border: 0;"></div>
						</div>

					</div>
					<div class="p1">
						<button @click="confim()">生成策略</button>
					</div>
					<div v-for="(item,index) in strategy_list" :key="index">
						<p class="hr"></p>
						<div class="note-i" >
								<div class="one" style="background-color: rgb(247,247,247);">
									<div style="color: rgb(77,77,77);width: 30%;">
										<span style="color: rgb(41,61,83);font-size: 18px;">{{item.symbol_deal}}/<span style="font-weight: 400;font-size: 12px;">{{item.symbol}}</span></span>
									</div>
									<div v-show="type == 2" style="font-size: 13px; color: rgb(19,168,24);" >多<span style="color: #000;">/</span> <span style="color: #bd1616;">空 </span><span style="color: #000;">-</span><span
										 style="margin-left: 2px;"> {{item.leverage}}X</span></div>
										 <div v-show="type == 3" style="font-size: 13px;"><span style="color: rgb(19,168,24);" v-show="item.up_down==1">多</span> <span style="color: #bd1616;" v-show="item.up_down==0">空 </span><span style="color: #000;">-</span><span
										 	 style="margin-left: 2px;"> {{item.leverage}}X</span></div>
									<div style="font-size: 12px;">账户权益: {{item.balance?(item.balance*1).toFixed(2):0}}USDT</div>
								</div>
							<div class="one one1">
								<div @click="toshowdetail(true)" style="margin: 0 20% 0 11%;display: flex;">持仓 <img src="../assets/1008.png" />
								</div>
								<div @click="toshowdetail()" style="display: flex;">交易记录 <img src="../assets/1008.png" /></div>
								<div style="flex: 1;text-align: center;">状态: <span style="color: #4389eb;">{{item.status == 1?'运行中':item.stop_type == 1?'临时停止':item.stop_type == 0?'待开启':item.stop_type == 8? '点卡不足':'待开启'}}</span></div>
							</div>
						<div v-show="type==2" class="two">
							<div class="p4">
								<p >建仓单数</p>
								<p >持仓数量</p>
								<P id="active" style="height: 50px;line-height: 50px;margin-bottom: 5px;border-bottom: 1px solid #eee;">持仓均价</P>
								<P id="active">交易笔数</P>
								
								<p id="active1">{{item.sell_count}}</p>
							</div>
							<div class="p2">
								<p>
									<span class="span"></span>
									 多: {{item.up_count?item.up_count:0}}<span style="margin-left: 2px;">单</span>
									 <span class="span1"></span>
									 </p>
								<p><span class="span"></span>多: {{item.up_filled_qty?item.up_filled_qty:0}} 张<span class="span1"></span></p>
								<p id="active"><span class="span"></span>多 <span class="span1"></span></p>
								<p id="active1"><span class="span" style="height: 30px;top: -10px;"></span>{{(item.up_price_avg*1).toFixed(2)}}<span style="margin-left: 2px;">USDT</span> <span class="span1" style="height: 30px;top: -10px;"></span></p>
								
								<P id="active" ><span class="span" ></span>实现收益<span class="span1" ></span></P>
								<p id="active1"><span class="span" style="height: 30px;top: -10px;"></span>{{(item.profit_cash*1).toFixed(2)}}<span style="margin-left: 2px;">USDT</span><span class="span1" style="height: 30px;top: -10px;"></span></p>
											
								
							</div>
							<div class="p3">
								<p>空: {{item.down_count?item.down_count:0}}<span style="margin-left: 2px;">单</span></p>
								<p>空: {{item.down_filled_qty?item.down_filled_qty:0}} 张</p>
								<p id="active">空 </p>
								<p id="active1">{{(item.down_price_avg*1).toFixed(2)}}<span style="margin-left: 2px;">USDT</span></p>
							<p id="active">收益率</p>
							<p id="active1">{{item.syl?item.syl.toFixed(2):0}}%</p>
							</div>
						</div>
						<div v-show="type==3" class="two">
							<div class="p4">
								<P id="active">建仓单数</P>
								<p id="active1">{{item.up_down == 0?'空:':'多:'}} {{item.buy_count}}</p>
								<P id="active">交易笔数</P>
								<p id="active1">{{item.sell_count}}</p>
							</div>
							<div class="p2">
								<P id="active" ><span class="span" ></span>持仓数量<span class="span1" ></span></P>
								<p id="active1"><span class="span" style="height: 30px;top: -10px;"></span>{{item.up_down == 0?'空:':'多:'}} {{item.buy_count_amount}}<span style="margin-left: 2px;">张</span><span class="span1" style="height: 30px;top: -10px;"></span></p>
								<P id="active" ><span class="span" ></span>持仓均价<span class="span1" ></span></P>
								<p id="active1"><span class="span" style="height: 30px;top: -10px;"></span>{{item.up_down == 0?'空:':'多:'}} {{item.buy_count_average}}<span style="margin-left: 2px;">USDT</span><span class="span1" style="height: 30px;top: -10px;"></span></p>
							</div>
							<div class="p3">
								<P id="active">实现收益</P>
								<p id="active1">{{item.profit_cash}} USDT</p>
								<P id="active">收益率</P>
								<p id="active1">{{item.syl?item.syl.toFixed(2):0}}%</p>
							</div>
						</div>
							<div class="button" style="padding-top: 7px;">
								<button  @click="fn5(item.id,item.stop_type)">删除策略</button>
								<button v-show="type == 3" :class="{'active':item.status == 1}" @click="id=item.id;close_now(item.up_down)">一键平仓</button>
								<button v-show="type == 2" :class="{'active':item.status == 1}" @click="show2 = true;id=item.id">一键平仓</button>
								<button  :class="{'active':item.status == 1}" @click="close_now2(item.id)">临时停止</button>
								<button :class="{'active':item.status == 0}" @click="open_strategy(item.id,'1',item.up_down)" >启动交易</button>
							</div>
						</div>
					</div>
					
					
				</div>

			</van-tab>
		</van-tabs>
		<van-dialog v-model="show2" title="选择多空" show-cancel-button :before-close="beforeClose">
			<div class="body-i">
				<van-checkbox v-model="checked2" style="font-size: 14px;">开多</van-checkbox>
				<van-checkbox v-model="checked3" style="font-size: 14px;">开空</van-checkbox>
			</div>

		</van-dialog>
	</div>
</template>

<script>
	import Vue from 'vue'
	import {
		Dialog,
		Checkbox
	} from 'vant';
	Vue.use(Dialog);
	Vue.use(Checkbox)
	export default {
		data() {
			return {
				active: 2,
				bourse: 1,
				symbol: 'BTC',
				monery: 5000,
				strategy_list: '',
				show2: false,
				checked2: true,
				checked3: true,
				up_down:0,
				id:'',
				type:3
			};
		},
		created() {
			let time = localStorage.getItem('time')
			clearInterval(time)
			this.start()
		},
		watch: {
			bourse(newvalue, oldvalue) {
				if(newvalue){
				}else{
					this.$toast.fail({
						message: '暂未开放',
						duration: 1200
					});
				}
				this.start()
			},
			type(newvalue, oldvalue){
				this.start()
			}
		},
		methods: {
			confim() {
				if(this.type == 3){
					this.show2 = true
				}else{
					this.getlist()
				}
			},
			getlist(){
				let obj = {};
					if (this.bourse == 1) {
						obj.bourse = 4
					}
					obj.symbol = 'USDT'
					obj.symbol_deal = this.symbol
					obj.amount = this.monery
					obj.type = this.type
					let arr = [1, 0];
					
					if(this.type == 3){
						if (this.checked2) {
							obj.up_down = '1'
						}
						if (this.checked3) {
							obj.up_down = '0'
						}
						if (this.checked3 && this.checked2) {
							obj.up_down = arr.join(",");
						}
					}else{
						obj.up_down = arr.join(",");
					}
					
					this.$axios.post(`/index/swapstrategy/set_strategy_all`, obj).then((res) => {
						if (res.data.code == 0) {
							this.$toast.success({
								message: res.data.msg,
								duration: 1200
							})
							setTimeout(() => {
								this.start();
							}, 1200)
						} else {
							this.$toast.fail({
								message: res.data.msg,
								duration: 1200
							});
						}
					})
			},
			start() {
				let str;
				if (this.bourse == 1) {
					str = 4
				}
				if (this.bourse == 0) {
					str = 1
				}
				let str1;
				if(this.type == 3){
					str1 ='get_strategy_list'
				}else{
					str1 = 'get_trend_strategy_list'
				}
				this.$axios
					.post(`/index/swapstrategy/${str1}`, {
						symbol: 'USDT',
						bourse: str,
						type: this.type,
					})
					.then((res) => {
						if (res.data.code == 0) {
							this.strategy_list = res.data.list;
						}
					});
			},
			fn5(id, type) {
				console.log(id, type);
				if (type == 2) {
					this.$toast.fail({
						message: "止盈停止后才可删除",
						duration: 2000
					});
					return;
				}
				Dialog.confirm({
					title: "提醒",
					message: "是否删除策略？",
				}).then(() => {
					this.$axios
						.post("/index/swap/delete_strategy", {
							strategy_id: id
						})
						.then((res) => {
							if (res.data.code == 0) {
								this.$toast.fail({
									message: "删除成功",
									duration: 2000
								});
								this.start();
							} else {
								this.$toast.fail({
									message: res.data.msg,
									duration: 2000
								});
							}
						});
				});
			},
			close_now(up_down) {
				Dialog.confirm({
				  title: "确定平仓",
				  message:
				    "是否要一键平仓？",
				})
				  .then(() => {
				    // on confirm
					let arr = [1, 0];
					
					if(this.type == 3){
						this.up_down = up_down
					}else{
						if (this.checked2) {
							this.up_down = '1'
						}
						if (this.checked3) {
							this.up_down = '0'
						}
						if (this.checked3 && this.checked2) {
							this.up_down = arr.join(",");
						}
					}
				  this.$axios
				  	.post("/index/swap/trend_clearance", {
				  		id:this.id,
						up_down: this.up_down
				  	})
				  	.then((res) => {
				  		if (res.data.code == 0) {
				  			this.$toast.success({
				  				message: res.data.msg,
				  				duration: 2000
				  			});
				  			this.start();
				  			this.closedeal = false;
				  		} else {
				  			this.$toast.fail({
				  				message: res.data.msg,
				  				duration: 2000
				  			});
				  			this.closedeal = false;
				  		}
				  	});
				  })
				  .catch(() => {
				    // on cancel
				  });
				
			},
			close_now2(id) {
				Dialog.confirm({
				  title: "确定停止",
				  message:
				    "是否要临时停止？",
				})
				  .then(() => {
				    // on confirm
				  this.$axios
				  	.post("/index/swap/pause_strategy", {
				  		id
				  	})
				  	.then((res) => {
				  		if (res.data.code == 0) {
				  			this.$toast.success({
				  				message: res.data.msg,
				  				duration: 2000
				  			});
				  			this.start();
				  		} else {
				  			this.$toast.fail({
				  				message: res.data.msg,
				  				duration: 2000
				  			});
				  		}
				  	});
				  })
				  .catch(() => {
				    // on cancel
				  });
				
			},
			open_strategy(s, i, b) {
				this.$axios
					.post("/index/swap/start_strategy", {
						id: s,
						up_down: b
					})
					.then((res) => {
						if (res.data.code == 0) {
							this.$toast.success({
								message: res.data.msg,
								duration: 2000
							});
							this.start();
							// this.strategy_list[i].status = 1
						} else {
							this.$toast.fail({
								message: res.data.msg,
								duration: 2000
							});
						}
					});
			},
			toshowdetail(bool = false) {
				let str;
				if (this.bourse == 1) {
					str = 4
				}
				if (this.bourse == 0) {
					str = 1
				}
				if (!this.bourse) {
					this.$toast.fail({
						message: "请选择交易所",
						duration: 2000
					});
					return;
				}
				if (bool) {
					// let abc = []
					// this.strategy_list.forEach(item => {
					// 	abc.push(item.symbol_deal)
					// })
					// abc = [...new Set(abc)].join(',')
					this.$router.push({
						name: '持仓',
						// params: {
						// 	bourse: str,
						// 	symbol: 'USDT',
						// 	stage: abc
						// }
					})
				} else {
					this.$router.push(
						"showdetail?bourse=" + str + "&type=1" + "&heyeu=1"
					);
				}
			},
			beforeClose: function(action, done) {
				if (action === "confirm") {
					if(this.type == 3){
						this.getlist()
					}else{
						this.close_now()
					}
					done(); // 关闭提示框
				} else if (action === "cancel") {
					done(); // 关闭提示框
				}
			}
		}
	};
</script>

<style lang="less" scoped>
	.body-i {
		display: flex;
		padding: 10px 20px;
		justify-content: space-around;
	}

	.note {
		min-height: 820px;
		font-size: 14px;background-color: rgb(247,247,247);
		padding-bottom: 100px;
	}

	.note-t {
		display: flex;
		margin: 4px 6px 0;
		border: 1px solid #eee;
		background-color: #fff;
		.div2 {
			display: flex;
			flex: 1;
			justify-content: space-around;
			// line-height: 26px;
			margin-top: 5px;
			margin-bottom: 5px;
			flex-flow: row wrap;
			div {
				// flex: 1;
				width: 80px;
				border: 1px solid #eee;
				text-align: center;
				font-size: 12px;
				color: rgb(130,130,130);
				border-radius: 4px;
				line-height: 20px;
			}

			.avtive {
				color: #fff;
				font-size: 12px;
				background:linear-gradient(to right,rgb(129,82,249),rgb(85,141,252));
			}
			
		}

		.div {
			line-height: 28px;
			width: 30%;
			display: flex;
            text-align: center;
			padding-left: 7%;
			justify-content: space-between;
			font-size: 13px;
			div {
				width: 2px;
				height: 16px;
				background-color: rgb(229,229,229);
				margin-top: 6px;
				margin-left: -5px;
			}
		}
	}

	.p1 {
		width: 100%;
		text-align: center;
		margin: 8px 0px 6px;
		border-bottom-right-radius:5px;
		border-bottom-left-radius:5px;
		button {
			width: 96%;
		background:linear-gradient(to right,rgb(129,82,249),rgb(85,141,252));
			color: rgb(235, 241, 245);
			border-radius: 4px;
			line-height: 34px;
		}
	}

	.open {
		padding: 0.3rem;
		font-size: 0.3rem;
	}

	.note-i {
		border-radius: 6px;
		border: 1px solid #eee;
		.one {
			display: flex;
			line-height: 36px;
			font-size: 13px;
			justify-content: space-around;
			width: 100%;
				text-align: center;
		}

		.one1 {
			div {
				// flex: 1;
				text-align: center;
				line-height: 32px;
				// font-size: 14px;
			}

			img {
				width: 11px;
				height: 11px;
				display: block;
				margin-top: 11px;
				margin-left: 2px;
			}

		}

		.two {
			display: flex;
			margin-bottom: 7px;
			padding: 0 .2rem;
			
			div {
				flex: 1;
				text-align: center;
				line-height: 25px;
				
				p{
					border: 1px solid #eee;
					margin-bottom: 4px;
				}
				div{
					border: 1px solid #eee;
					margin-bottom: 4px;
				}
			}
			.p4{
				p{
					border-right: 0;
				}
			}
			.p2{
				p{
					position: relative;
					border-left: 0;
					border-right: 0;
					.span{
						position: absolute;
						width: 1px;
						height: 18px;
						background-color: rgb(229,229,229);
						top: 3px;
						left: 0;
					}
					.span1{
						position: absolute;
						width: 1px;
						height: 18px;
						background-color: rgb(229,229,229);
						top: 3px;
						right: 0;
					}
				}
			}
			.p3{
				p{
					border-left: 0;
				}
			}
			#active{
				border-bottom: 0;
				background-color: rgb(247,247,247);
				margin-bottom: 0;
				padding-top: 6px;
			}
			#active1{
				border-top: 0;
				background-color: rgb(247,247,247);
			}
		}

		.button {
			width: 100%;
			text-align: right;
			margin-bottom: 8px;
			display: flex;
			justify-content: space-around;
			button {
				font-size: 12px;
				line-height: 20px;
				color: rgb(199, 199, 199);
				border: 1px solid rgb(188, 187, 193);
				border-radius: 3px;
				background-color: #FFFFFF;
				// padding:  0 8px;
				// margin: 2%;
			}
			.active{
				border: 0;
				background-color: rgb(100,123,253);
				color: #FFFFFF;
				border: 0;
			}
		}
	}

	.tolead {
		display: flex;
		justify-content: space-between;
		height: 1rem;
		line-height: 1rem;
		padding: 0 0.3rem;
		background: #fff;

		p {
			font-weight: bold;
		}

		img {
			width: 0.24rem;
			margin-right: 0.2rem;
		}
	}
	.van-tabs__nav{
		margin: 0;
	}
	/deep/.van-tabs__nav--card{
		margin: 0 10px;
		border-top-right-radius:5px;
		border-top-left-radius:5px;
	}
</style>
