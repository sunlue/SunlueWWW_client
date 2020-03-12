<template>
  <div class="right-btn-box" :class="{'right-btn-box-page':!isHome,'right-btn-box-green':isGreen}">
    <div class="list">
      <div class="text"><a :href="'tel:'+tel">{{tel}}</a></div>
      <div class="icon">
        <img src="~/assets/images/shortcut1.png" alt="">
      </div>
    </div>
    <div class="list">
      <div class="text"><a :href="'tencent://message/?uin='+QQ">在线客服</a></div>
      <div class="icon">
        <img src="~/assets/images/shortcut2.png" alt="">
      </div>
    </div>
    <div class="list">
      <div class="code">
        <img :src="code" alt="">
      </div>
      <div class="icon">
        <img src="~/assets/images/shortcut3.png" alt="">
      </div>
    </div>
    <div class="list">
      <div class="icon" :class="{'icon-rotate':topBtn}" @click="goUp()">
        <img src="~/assets/images/shortcut4.png" alt="">
      </div>
    </div>
  </div>
</template>
<style src="~/assets/css/rightBtn.css" scoped></style>
<script>
  export default {
    props: {
      isHome: Boolean,
      isGreen: Boolean
    },
    data() {
      return {
        tel: "400-0304-517",
        QQ: "2746256648",
        code: require("assets/images/QRcode.png"),
        topBtn: false
      }
    },
    methods: {
      goUp() {
        let self = this;
        let nowTop = document.body.scrollTop || document.documentElement.scrollTop; // 获取当前滚动条位置
        if (nowTop > 0) {
          window.requestAnimationFrame(self.goUp);
          window.scrollTo(0, nowTop - (nowTop / 5));
        }
      },
      handleScroll() {
        let self = this;
        let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
        let screenHeight = document.documentElement.clientHeight;
        self.topBtn = scrollTop > screenHeight;
      }
    },
    mounted() {
      window.addEventListener("scroll", this.handleScroll) || document.addEventListener("scroll", this.handleScroll);
      this.handleScroll();
    },
    destroyed() {
      window.removeEventListener('scroll', this.handleScroll) || document.removeEventListener('scroll', this.handleScroll)
    }
  }
</script>
