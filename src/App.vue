<template>
  <div id="app">
    <div class="outter">
      <!--通用头部-->
      <header id="header" v-if="this.$store.state.headerStatus">
        <!-- <wx-header :pageName="pageName"></wx-header>23423423423423423432 -->
        <div class="portrait">
          <img :src="this.touxiang" alt v-if="!this.$store.state.tishi">
          <i class="iconfont" v-if="this.$store.state.tishi">&#xe61a;</i>
        </div>
        <h3 class="h3">
          <span v-if="this.$store.state.tishi">HGH</span>
          <span v-if="!this.$store.state.tishi">{{ this.username }}</span>
        </h3>
        <span class="iconfont icon">&#xe631;</span>
      </header>
      <!--搜索框 只在“微信”和“通讯录”页面下显示-->
      <!--四个门面页 “微信” “通讯录” “发现” “我”-->
      <section class="app-content">
        <transition
          name="custom-classes-transition"
          :enter-active-class="enterAnimate"
          :leave-active-class="leaveAnimate"
        >
          <router-view/>
        </transition>
      </section>
      <!--底部导航 路由 -->
      <footer class="app-footer" :v-if="$store.state.footstatus">
        <wx-nav></wx-nav>
      </footer>
    </div>
  </div>
</template>
<script>
import mixin from "./mixin"; // 混合被单独放在 mixin.js 中管理
window.mixin = mixin; // 将 混合/mixin 暴露在窗口对象中，某些组件需要时，直接提取 window.mixin
import WxHeader from "@/components/common/wx-header";
import WxNav from "@/components/common/wx-nav";

export default {
  name: "app",
  components: {
    WxHeader,
    WxNav
  },
  data() {
    return {
      pageName: "",
      routerAnimate: false,
      enterAnimate: "", //页面进入动效
      leaveAnimate: "", //页面离开动效
      username: "",
      touxiang: ""
    };
  },
  created() {},
  mounted() {
    this.username = localStorage.getItem("username");
    this.touxiang = localStorage.getItem("avatar");
  },
  methods: {
    tap() {
      this.$router.go(-1);
    }
  },
  watch: {
    // 监听 $route 为店内页设置不同的过渡效果
    $route(to, from) {
      const toDepth = to.path.length;
      const fromDepth = from.path.length;
      if (toDepth === 2) {
        this.$store.commit("setPageName", to.name);
      }
      //同一级页面无需设置过渡效果
      if (to.meta.Athesamelevel) {
        return;
      }
      // this.enterAnimate =
      //   toDepth > fromDepth ? "animated fadeInRight" : "animated fadeInLeft";
      // this.leaveAnimate =
      //   toDepth > fromDepth ? "animated fadeOutLeft" : "animated fadeOutRight";
    }
  },
  updated() {
    // 只要组件发生变化都会触发可用于切换组件时公共数据的刷新
    // this.$store.dispatch('newbalance')
  }
};
</script>


<style lang="scss">
$material-icons-font-path: "~material-icons/iconfont/";
@import "material-icons/iconfont/material-icons.scss";
@import "./assets/css/toast.css";
@import "assets/css/base.css";
@import "assets/css/common.css";
@import "assets/css/wx-header.css";
/*阿里 fonticon*/
@import "assets/css/lib/iconfont.css";
/*过渡效果需要的动画库*/
@import "assets/css/lib/animate.css";
/*weui 样式库 非常适合高仿微信*/
@import "assets/css/lib/weui.min.css";

@import "./assets/font/iconfont.css";
@font-face {
  font-family: "iconfont";
  src: url("./assets/font1/iconfont.woff") format("woff");
}
.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
body {
  height: 100%;
  width: 100%;
  background: #f3f3f3;
}
#header {
  background: #12377d;
  height: 0.88rem;
  width: 100%;
  text-align: center;
  line-height: 0.88rem;
  font-size: 0.36rem;
  color: #ffffff;
  position: fixed;
  z-index: 999;
}
.l-span1 {
  float: left;
  margin-left: 0.3rem;
}
.portrait {
  height: 0.7rem;
  width: 0.7rem;
  background: #cfcfcf;
  border-radius: 50%;
  margin-left: 0.38rem;
  border: 2px solid #4464a1;
  margin-top: 0.1rem;
  overflow: hidden;
  text-align: center;
  float: left;
  line-height: 0.7rem;
}
.h3 {
  color: #fcfffd;
  float: left;
  margin-left: 0.18rem;
}
.icon {
  float: right;
  margin-right: 0.4rem;
  font-size: 0.46rem;
  margin-top: 0.06rem;
}
.portrait > img {
  height: 100%;
  width: 100%;
  margin-top: -5px;
}
</style>
