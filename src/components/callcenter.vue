<template>
  <div class="callcenter" style="background:linear-gradient(to right,rgb(129,82,249),rgb(85,141,252));font-size: 14px;min-height: 815px;">
    <div class="tophader" onclick="window.history.go(-1)">
      <van-icon name="arrow-left" />
      <p>联系客服</p>
    </div>
	<div class="my-b">
		<img  class="img" :src="infouser.avatar" alt 
		 />
		<p class="bigtitle">您的专属微信客服</p>
		<p class="somot">7x24小时为您提供客服服务</p>
		<p class="bigtitle " style="margin-top: 25px;font-size: 15px;">扫描下方二维码添加</p>
		<p class="bigtitle" style="font-size: 15px;">您的专属客服微信</p>
		<p class="bigtitle" style="font-size: 15px;">支持安卓/苹果</p>
		<img class="img1" :src="info.wx" alt />
		<!-- <img class="img1" src="../assets/icon-wx@2x.png" alt /> -->
	</div>
   <!-- <div class="contestdiv">
      <p class="bigtitle">您的专属微信客服</p>
      <p class="somot">7x24小时为您提供客服服务</p>
      <div class="borders">
        <div class="flexs">
          <p class="qimg">
            <img src="../assets/icon-wx@2x.png" alt />
          </p>
          <div>
            <p class="one">您的专属客服</p>
            <p class="two">7x24小时为您提供客服服务</p>
          </div>
        </div>
        <div id="qrcodes">
          <img :src="info.wx" alt />
        </div>
        <p class="shoms">使用微信识别添加客服</p>
      </div>
    </div> -->
  
  </div>
</template>

<script>
export default {
  data() {
    return {
      info: {},
	  infouser: {},
    };
  },
  created() {
    this.$axios.post("/index/member/getService").then(res => {
      if (res.data.code == 0) {
        this.info = res.data.info;
      }
    });
	this.$axios.post("/index/member/getUserInfo").then(res => {
		if (res.data.code == 0) {
			this.infouser = res.data.info;
		}
	});
  },
  mounted() {},
  methods: {
    copy(s) {
      var clipboard = new this.$Clipboard(".copyq", {
        text: () => {
          return s;
        }
      });
      clipboard.on("success", () => {
        this.$toast.success({ message: "复制成功", duration: 1200 });
        // 释放内存
        clipboard.destroy();
      });
      clipboard.on("error", () => {
        // 不支持复制
        this.$toast.fail({ message: "该浏览器不支持自动复制", duration: 1200 });
        // 释放内存
        clipboard.destroy();
      });
    }
  }
};
</script>

<style lang="less" scoped>
.tophader {
  p {
    font-size: 0.36rem;
  }
}
	.my-b {
		width: 90%;
		margin: 60px auto 0;
		border-radius: 6px;
		z-index: 100;
		padding: 35px 0 30px;
		background-color: #fff;
		text-align: center;
		position: relative;
		.img{
			width: 72px;
			    height: 72px;
			    border-radius: 50%;
			    position: absolute;
			    top: -42px;
			    border: 3px solid rgb(114, 104, 255);
			    left: 39%;
		}
		.img1{
			margin-top: 25px;
			height: 160px;
			width: 160px;
		}
		}
.contestdiv {
  padding: 0.3rem;
}
.bigtitle {
  font-size: 0.36rem;
  column-rule: #222222;
}
.somot {
  font-size: 0.24rem;
  color: #999999;
}
.borders {
  border: 1px solid #eeeeee;
  padding: 0.4rem;
  margin: 0.2rem auto;
  .flexs {
    display: flex;
  }
  .one {
    font-size: 0.32rem;
    color: #222222;
  }
  .two {
    font-size: 0.2rem;
    color: #999999;
  }
  .shoms {
    font-size: 0.24rem;
    text-align: center;
    color: #b9bdc0;
  }
}
.qimg {
  margin-right: 0.3rem;
  img {
    width: 0.8rem;
  }
}
.qnumber {
  margin-top: 0.1rem;
}
.wxrefer {
  margin: 0.1rem auto;
}
.wximg img {
  width: 0.77rem;
}
button {
  width: 2rem;
  height: 0.66rem;
  background: #3c8cff;
  border-radius: 33px;
  color: white;
  font-size: 0.3rem;
  text-align: center;
  display: block;
  margin: 0.3rem auto 0.6rem;
}
.callcenter {
  #qrcodes {
    height: 3.97rem;
    width: 3.97rem;
    margin: 0.2rem auto;
    text-align: center;
    img {
      height: 100%;
    }
  }
}
</style>
