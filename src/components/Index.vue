<template>
  <div class="index">
    <van-swipe :show-indicators="false" :autoplay="4000" indicator-color="white">
      <van-swipe-item v-for="(item,i) in info" :key="i">
        <div class="boxall">
          <div class="box" style="height: 180px;">
            <img :src="url + item.pic" alt style="height: 100%;width: 100%;"/>
          </div>
        </div>
      </van-swipe-item>
    </van-swipe>

    <div class="gonite">
      <img src="../assets/12-2-1.png" alt />

      <div class="gobox" style="height: .4rem;
    width: 84%;
    overflow: hidden;">
        <marquee color="#333" background="none" class="vanbar" v-if="list.length>0">
          <span @click="$router.push('noticedateil?id='+list[0].article_id)">{{list[0].title}}</span>
        </marquee>
		
      </div>
		<img  @click="$router.push('more')" src="../assets/12-2-3.png"style="margin-right: 0;"  alt />
    </div>
	<div class="bootom">
		
		<div class="body" style="font-size: 14px;color: rgb(150,159,158);padding: .1rem 0 .1rem .1rem;">
			<div>名称 <!-- <van-icon v-show="active!=1" @click="active=1" name="play" color="rgb(150,159,158)" style="transform: rotate(270deg);position: absolute;top: 2px;left: 30px;" size="11"/> --> 
			<!-- <van-icon  v-show="active==1"  @click="active=0" name="play" color="#d04a62" style="transform: rotate(270deg);position: absolute;top: 2px;left: 30px;" size="11"/> -->
			<!-- <van-icon  v-show="active!=2" @click="active=2" name="play" color="rgb(150,159,158)" style="transform: rotate(90deg);position: absolute;top: 8px;left: 30px;" size="11"/> --> <span style="margin-left: 3px;">/ 24H量</span> 
			<!-- <van-icon  v-show="active==2"  @click="active=0" name="play" color="#d04a62" style="transform: rotate(90deg);position: absolute;top: 8px;left: 30px;" size="11"/> -->
			<!-- <van-icon v-show="active!=3"  @click="active=3" name="play" color="rgb(150,159,158)" style="transform: rotate(270deg);position: absolute;top: 2px;left: 93px;" size="11"/> -->
			<!-- <van-icon  v-show="active==3"  @click="active=0" name="play" color="#d04a62" style="transform: rotate(270deg);position: absolute;top: 2px;left: 93px" size="11"/> -->
			<!-- <van-icon v-show="active!=4"  @click="active=4" name="play" color="rgb(150,159,158)" style="transform: rotate(90deg);position: absolute;top: 8px;left: 93px;" size="11"/> -->
			<!-- <van-icon  v-show="active==4"  @click="active=0" name="play" color="#d04a62" style="transform: rotate(90deg);position: absolute;top: 8px;left: 93px" size="11"/> -->
			</div>
			<div style="text-align: right;margin-left: 25%;">最新价
			<!-- <van-icon v-show="active!=5"  @click="active=5" name="play" color="rgb(150,159,158)" style="transform: rotate(270deg);position: absolute;top: 2px;right: -14px;" size="11"/>
			<van-icon  v-show="active==5"  @click="active=0" name="play" color="#d04a62" style="transform: rotate(270deg);position: absolute;top: 2px;right: -14px;;" size="11"/>
			<van-icon v-show="active!=6"  @click="active=6" name="play" color="rgb(150,159,158)" style="transform: rotate(90deg);position: absolute;top: 8px;right: -14px;" size="11"/>
			<van-icon  v-show="active==6"  @click="active=0" name="play" color="#d04a62" style="transform: rotate(90deg);position: absolute;top: 8px;right: -14px;;" size="11"/> -->
			</div>
			<div style="margin-right: 15px;">涨跌幅
			<!-- <van-icon v-show="active!=7"  @click="active=7" name="play" color="rgb(150,159,158)" style="transform: rotate(270deg);position: absolute;top: 2px;left: 43px;" size="11"/>
			<van-icon  v-show="active==7"  @click="active=0" name="play" color="#d04a62" style="transform: rotate(270deg);position: absolute;top: 2px;left: 43px;" size="11"/>
			<van-icon v-show="active!=8"  @click="active=8" name="play" color="rgb(150,159,158)" style="transform: rotate(90deg);position: absolute;top: 8px;left: 43px;" size="11"/>
			<van-icon v-show="active==8"  @click="active=0" name="play" color="#d04a62" style="transform: rotate(90deg);position: absolute;top: 8px;left: 43px;" size="11"/> -->
			</div>
		</div>
		<div v-show="isshow2" class="body" v-for="(item,index) in list4" style="padding: 0.19rem 0.17rem 0.19rem 0.1rem;" :key="index">
			<div class="one" style="flex: 1;"><span style="font-weight: 550;margin: 0;">{{item.symbol}}</span> <span style="color: #999999;font-size: 12px;margin-left: 5px;">/USDT</span>
			<p>24H量 {{item.amount?parseInt(item.amount):0}}张</p> </div>
			<div class="two" style="flex: 1;text-align: right;margin-right: 20px;margin-top: 9px;">{{item.close?item.close:0}} </div>
			<div v-show="item.up_or_down<0" class="three active" >{{item.up_or_down?item.up_or_down.toFixed(2):0}}%</div>
			<div v-show="item.up_or_down>0" class="three" style="display: flex;"><div style="font-size: 12px;margin-left: 13%;height: 27px;line-height: 27px;">+</div> <span style="">{{item.up_or_down.toFixed(2)}}%</span> </div>
		</div>
	</div>
	
  </div>
</template>

<script>
export default {
  data() {
    return {
      info: [],
      list: [],
      arr:[],
      newlink:'',
	  isshow:true,
	  isshow2:true,
	  movielist:{},
	  bidui:false,
	  list3:[],
	  list4:[],
	  time:'',
	  url:'',
	  active:0
    };
  },
  created() {
	  const env = process.env.NODE_ENV;
	  if(env==='production'){  // 生产环境
	  }else{  // 本地环境
	  this.url = 'http://192.168.0.104:81'
	  }
    this.$axios
      .get("/index/article/image", { page: 1, limit: 10 })
      .then(res => {
        this.info = res.data.info;
      });

    this.$axios
      .get("/index/article/newArt", { page: 1, limit: 1 })
      .then(res => {
        this.list = res.data.info;
      });

   

	  this.$axios
	  .get("/index/rank/get_market")
	  .then(res => {
		  if(res){
			  this.list4 = res.data.data
			  this.fn1()
		  }
	  		
	  });
	
	
  },
  updated() {},
  beforeDestroy() {
  	clearInterval(this.time)
	this.time = null
  },
  watch:{
	  active(oldvalue,newvalue){
		  console.log(oldvalue)
	  }
  },
  methods: {
	  fn1(){
		  clearInterval(this.time)
		  this.time = setInterval(()=>{
		  		  this.$axios
		  		  .get("/index/rank/get_market")
		  		  .then(res => {
					  this.list4 = res.data.data
		  		  });
		  },10000)
		  localStorage.setItem('time',JSON.stringify( this.time))
	  },
	  fn3(bool){
	  		  if(bool){
	  			  this.$toast.fail({ message: '该功能呢暂未开放,敬请期待', duration: 1200 });
	  		  }else{
	  			  this.$toast.fail({ message: '钱包维护中,请联系客服', duration: 1200 });
	  		  }
	  },
	  tobidui(){
		  this.bidui = true
		  this.$axios
		    .post("/index/strategy/get_symbol", {
		      symbol: 'USDT',
		      bourse: 1
		    })
		    .then(res => {
		    
		  	  this.list3 = res.data.symbol
		  	  
		   
		  	})
	  },
    linkmore(){
      location.href=this.newlink;
    }
  }
};
</script>

<style lang="less" scoped>
.note {
  overflow: hidden;
}
.note .van-swipe {
  margin-top: 0;
  height: 3.18rem;
}
.note .van-swipe .box {
  display: flex;
  justify-content: space-between;
}
.note .van-swipe .boxall {
  padding: 0rem;
}

.tophader {
  img {
    position: unset;
        height: 0.4rem;
    width: auto;
    margin-top: 0.3rem;
  }
}
.gonite {
	border-top: 1px solid #eee;
	height: 32px;
	border-bottom: 1px solid #eee;
  padding: 0.1rem 0.2rem .1rem .3rem;
  height: 0.4rem;
  overflow: hidden;
  display: flex;
  margin: 2px 0;
  .gobox {
    display: flex;
  }
  span {
    color: #333333;
    margin-right: 2rem;
    font-size: .25rem;
    font-weight: bold;
  }
  img {
    height: 0.32rem;
    position: relative;
    margin-right: 0.2rem;
    top: 50%;
    margin-top: -0.17rem;
  }
  .vanbar {
    width: 100%;
    height: 100%;
    line-height: 0.4rem;
    padding: 0;
    font-size: 0.24rem;
  }
}
.trading {
  display: flex;
  justify-content: space-between;
  background: #f6f7f9;
  padding-top: 0.15rem;
  .left {
    width: 59%;
    background: url("../assets/title1.png") no-repeat;
    background-size: 100%;
    height: 1.4rem;
  }
  .right {
    width: 39%;
    line-height: 1.4rem;
    img {
      width: 0.37rem;
      margin: 0 0.3rem 0;
      position: relative;
      top: 0.05rem;
    }
    span {
      font-size: 0.3rem;
      font-weight: bold;
    }
  }
  .one {
    font-size: 0.3rem;
    padding: 0.3rem 0.3rem 0.1rem;
  }
  .two {
    font-size: 0.24rem;
    color: #999999;
    padding-left: 0.3rem;
  }
  div {
    background: #fff;
  }
}
.tradingtwo {
  .left {
    background: url("../assets/ssmal.png") no-repeat;
    background-size: 100%;
  }
}
.lastnews {
  display: flex;
  padding: 0.3rem;
  justify-content: space-between;
  .onew {
    color: #020202;
    font-size: 0.3rem;
    position: relative;
    top: 0.05rem;
    font-weight: bold;
    img {
      width: 0.76rem;
      height: auto;
      position: relative;
      top: 0.02rem;
    }
  }
  .gobox {
    font-size: 0.24rem;
    line-height: 0.4rem;
    position: relative;
    top: .05rem;
  }
  .more {
    color: #bababa;
    font-size: 0.24rem;
    img {
      width: 0.2rem;
      height: 0.4rem;
      position: relative;
      top: 0.02rem;
    }
    i {
      position: relative;
      font-size: 0.3rem;
      top: 0.06rem;
    }
  }
}
.imgbox {
  padding: 0.2rem 0.3rem;
  img {
    width: 100%;
  }
}
.nestli {
  margin-bottom: .2rem;
}
.nestli:last-child{
  padding-bottom: 1rem;
}
.newtest {
  display: flex;
  justify-content: space-between;
  width: 94%;
  margin: auto;
  padding-bottom: 0.2rem;
  margin-bottom: 0.2rem;
  border-bottom: 1px solid #f7f7f7;
  p {
    font-size: 0.24rem;
    line-height: 0.35rem;
  }
  div {
    padding: 0.1rem;
  }
  .left {
    width: 55%;
    position: relative;
  }
  .right {
    width: 34%;
    background: url("../assets/news.png") no-repeat;
    background-size: 100% 100%;
    color: #fff;
    display: flex;
    height: 1.32rem;
  }
  .spanr {
    width: 0.06rem;
    height: 0.65rem;
    background: #fff;
    position: relative;
    top: 50%;
    margin: -0.325rem 0.2rem 0;
  }
  .gao {
    font-size: 0.24rem;
    color: #bababa;
    // margin-top: 0.1rem;
    line-height: 0.4rem;
    span {
          width: 100%;
      color: #000;
      font-size: 0.28rem;
      margin-right: 0.1rem;
      padding-bottom: .12rem;
      position: relative;
    }
  }
}

.goboxend{
  span{
      margin-right: .6rem;
      color: #333;
      font-size: .25rem;
      font-weight: bold;
  }
}
.index-icon{
	display: flex;
	padding: .2rem  1%;
	div{
		flex: 1;
		height: .87rem;
		text-align: center;
		img{
			width: 20px;
			height: 23px;
		}
		p{
			margin-top: 2px;
			font-size: 12px;
			color: #000000;
		}
	}
}
.index-img{
	margin-top: 6px;
	img{
		width: .35rem;
		height: .35rem;
		margin-right: .4rem;
		margin-top: .2rem;
	}
}
.index-body{
	padding: .24rem .24rem;
	display: flex;
	.item{
		flex: 1;
		text-align: center;
		div{
			font-size: 0.28rem;
			color: #16181A;
		}
		p{
			color: #25a67e;
			font-size: .35rem;
			span{
				font-size: 12px;
				background-color: rgb(188,218,252);
				padding: .05rem .09rem;
			}
			margin: .1rem 0;
		}
		.one{
			font-size: 15px;
			color: #000000;
		}
	}
	.active{
		p{
			color: #25a67e;
			span{
				background-color: rgb(252,222,224);
			}
		}
	}
}
	
.bootom{
	padding-left: .2rem;
	padding-right: .2rem;
	margin-bottom: 50px;
	.title{
		font-weight: bold;
		display: flex;
		padding:.2rem 0;
		position: relative;
		.active{
			border-bottom: 2px solid rgb(38,136,251);
		}
		img{
			width: 16px;
			height: 16px;
			position: absolute;
			right: 13px;
			top: 13px;
		}
	}
	.body{
		display: flex;
		justify-content: space-between;
		color: rgb(96,94,95);
		padding: .1rem .3rem .1rem .1rem;
		// border-bottom: 1.3px solid #EEEEEE;
		div{
			position: relative;
			span{
				margin-left: 2px;
			}
		}
		.one{
			font-size: 15.5px;
			color: #000000;
			// font-weight: 550;
			p{
				font-size: 12px;
				font-weight: 540;
				color: rgb(169,167,168);
				margin-top: 4px;
			}
		}
		.two{
			p{
				font-weight: 540;
				font-size: 12px;
				color: rgb(169,167,168);
				margin-top: 4px;
			}
		}
		.three{
			color: #FFFFFF;
			background-color: rgb(0,191,137);
			margin-top: 5px;
			height: 27px;
			width: 65px;
			line-height: 27px;
			text-align: center;
			font-size: 13px;
			border-radius: 4px;
		}
		.active{
			color: #FFFFFF;
			background-color: rgb(208,74,98)
			
		}
		.active1{
			color: rgb(208,74,98);
			
		}
	}
}
.poup {
  p {
    border-bottom: 1px solid #e5e5e5;
  }
 
}
.poup-title{
	display: flex;
	position: relative;
	padding: .2rem;
	border-bottom: 4px solid #E5E5E5;
	div{
		margin-left: .4rem;
	}
	.item{
		color: rgb(34, 132, 253);
		margin-left: .7rem;
		font-size: 15px;
	}
	.about{
		background-color: rgb(34, 132, 253);
		width: 3px;
		height: 17px;
		position: absolute;
		left: 0px;
		top: 12px;
	}
	}
	.poup-body{
		width: 100%;
		display: flex;
		padding: .2rem;
		border-bottom: 1px solid #E5E5E5;
		
		span{
			flex: 1;
			font-size: 16px;
			text-align: center;
		}
	}
	.poup .li-item{
		display: flex;
		font-size: 16px;
		padding: 0.3rem;
		    border-bottom: 1px solid #e5e5e5;
		div{
			flex: 1;
			font-weight: 500;
			text-align: center;
		}
		
	}
	.index-img1{
		width: 30px;
		height: 30px;
		display: block;
		margin-right: 9px;
		margin-top: 5px;
	}
	
</style>
