<template>
  <div id="app">
      <tou></tou>
      <div class="fiexbddiv"></div>

      <div class="main">
        <router-view></router-view>
      </div>
   
      <div class="footer">
     Copyright © 2019 - guoang个人博客 -    <a style="color: #858585;" href="http://www.miit.gov.cn/" target=“_blank”>陕 ICP 备 19016483 号-1</a>
    - <a style="color: #858585;" href="http://www.guoang.xyz/" target=“_blank”> www.guoang.xyz</a>
    </div>

    <div class="gotitle" v-if="btnFlag" @click="backTop()">
      <img src="./assets/gotop.png" alt />
    </div>
  </div>
</template>

<script>
import tou from "./components/tou.vue";
export default {
  components: { tou },
  methods: {},
  name: "App",
  data() {
    return {
      btnFlag: false
    };
  },
  // vue的两个生命钩子。
  // window对象，所有浏览器都支持window对象。它表示浏览器窗口，监听滚动事件
  mounted() {
    window.addEventListener("scroll", this.scrollToTop);
  },
  destroyed() {
    window.removeEventListener("scroll", this.scrollToTop);
  },

  methods: {
    // 点击图片回到顶部方法，加计时器是为了过渡顺滑
    backTop() {
      let that = this;
      let timer = setInterval(() => {
        let ispeed = Math.floor(-that.scrollTop / 5);
        document.documentElement.scrollTop = document.body.scrollTop =
          that.scrollTop + ispeed;
        if (that.scrollTop === 0) {
          clearInterval(timer);
        }
      }, 16);
    },

    // 为了计算距离顶部的高度，当高度大于200显示回顶部图标，小于200则隐藏
    scrollToTop() {
      let that = this;
      let scrollTop =
        window.pageYOffset ||
        document.documentElement.scrollTop ||
        document.body.scrollTop;
      that.scrollTop = scrollTop;
      if (that.scrollTop > 200) {
        that.btnFlag = true;
      } else {
        that.btnFlag = false;
      }
    }
  }
};
</script>

<style>
.main {
  margin-top: 100px;
}
body {
  padding: 0; /*去除内边距*/
  border: 0; /*去除边框*/
  margin: 0; /*去除外边距*/
  position: relative;
}
.footer {
  background-color:#2e2e2e;
  float: left;
  bottom: 0;
  min-height: 40px;
  line-height: 40px
}
.gotitle {
  position: fixed;
  right: 7%;
  top: 70%;
  color: aliceblue;
  z-index: 9999;
}
</style>
