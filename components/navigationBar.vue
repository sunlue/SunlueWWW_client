<template>
  <div>

    <div class="nav-box pc-nav clearfix"
         :class="{'nav-box-position':navStatus&!isWhite&!isAbout,'nav-box-white':isWhite,'nav-box-about':isAbout}">
      <div class="logo float-left">
        <n-link to="/">
          <img v-if="!isWhite" src="~/assets/images/logo.png" alt="">
          <img v-else src="~/assets/images/logoWhite.png" alt="">
        </n-link>
      </div>
      <ul class="nav float-right">
        <li>
          <n-link to="/tourism">智慧文旅</n-link>
        </li>
        <li>
          <n-link to="/agriculture">智慧农业</n-link>
        </li>
        <li>
          <n-link to="/operation">社会化运营</n-link>
        </li>
<!--        <li>-->
<!--          <a href="javascript:void(0)">客户中心</a>-->
<!--        </li>-->
        <li>
          <n-link to="/about">关于我们</n-link>
        </li>
      </ul>
    </div>
    <div class="mobile-nav" :class="{'mobile-nav-show':mobileShow}">
      <div class="logo">
        <n-link to="/">
          <img class="img-fluid" src="~/assets/images/logo.png" alt="">
        </n-link>
      </div>
      <div class="nav-btn" @click="mobileShow=!mobileShow">

      </div>
      <div class="mobile-nav-main">
        <div class="box">
          <ul class="mobile-list-box">
            <li>
              <n-link to="/tourism">智慧文旅</n-link>
            </li>
            <li>
              <n-link to="/agriculture">智慧农业</n-link>
            </li>
            <li>
              <n-link to="/operation">社会化运营</n-link>
            </li>
<!--            <li>-->
<!--              <a href="javascript:void(0)">客户中心</a>-->
<!--            </li>-->
            <li>
              <n-link to="/about">关于我们</n-link>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    props: {
      isWhite: Boolean,
      isAbout: Boolean
    },
    methods: {
      handleScroll() {
        let self = this;
        let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
        this.navStatus = !!scrollTop;
      }
    },
    data() {
      return {
        navStatus: false,
        mobileShow: false
      }
    },
    mounted() {
      window.addEventListener("scroll", this.handleScroll) || document.addEventListener("scroll", this.handleScroll);
    },
    destroyed() {
      window.removeEventListener('scroll', this.handleScroll) || document.removeEventListener('scroll', this.handleScroll)
    }
  }
</script>
<style src="~/assets/css/navigationBar.css" scoped></style>
