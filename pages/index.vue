<template>
  <div class="home-box">
    <div class="bg-box">
      <Nav/>
      <Shortcut :isHome="true"/>
      <section class="home-banner">
        <div class="banner-box">
          <div class="swiper-container" id="homeBanner">
            <div class="swiper-wrapper">
              <div class="swiper-slide" v-for="item in homeBanner">
                <div class="img-box">
                  <img :src="item.url" alt="">
                </div>
                <div class="content-box">
                  <p class="title ani" swiper-animate-effect="fadeInUp" swiper-animate-duration="0.5s"
                     swiper-animate-delay="0s">{{item.title}}</p>
                  <p class="intro ani" swiper-animate-effect="fadeInUp" swiper-animate-duration="0.5s"
                     swiper-animate-delay="0.5s" v-html="item.intro"></p>
                </div>
              </div>
            </div>
          </div>
          <div class="swiper-pagination home-banner-pagination"></div>
          <div class="home-banner-total">
            <span class="current">{{homeBannerCurrent}}</span>
            <span class="separator">/</span>
            <span class="total">{{homeBanner.length>10?homeBanner.length:'0'+homeBanner.length}}</span>
          </div>
        </div>
      </section>

      <section class="tourism-box">
        <div class="content-box">
          <div class="title-box">
            <p class="title">智慧文旅</p>
            <p class="title-en">intelligence tourism</p>
          </div>
          <div class="container-fluid">
            <div class="row">
              <div class="col list-box " v-for="item in tourismData">
                <p class="title">{{item.title}}</p>
                <ul class="list-inline">
                  <li class="list-item" v-for="data in item.data">
                    {{data}}
                  </li>
                </ul>
                <span class="icon-box"></span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="agriculture-box">
        <div class="content-box clearfix">
          <div class="float-left text-box">
            <p class="title">{{agriculture.title}}</p>
            <p class="title-en">{{agriculture.titleEn}}</p>
            <p class="intro">{{agriculture.intro}}</p>
          </div>
          <div class="float-left list-box">
            <div class="container-fluid">
              <div class="row">
                <div class="col-lg-6 list-item" v-for="item in agriculture.data">
                  <n-link to="/">
                    <div class="img-box">
                      <img :src="item.image" alt="">
                    </div>
                    <p class="name">{{item.name}}</p>
                  </n-link>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="more-box">
          <div class="text-box">
            <div class="show-box">
              <n-link to="/agriculture">
                <p class="text text-en">MORE</p>
                <p class="text">了解更多</p>
              </n-link>
            </div>
          </div>
        </div>
      </section>

      <section class="operation-box">
        <div class="content-box clearfix">
          <div class="float-left text-box">
            <p class="title">{{operation.title}}</p>
            <p class="title-en">{{operation.titleEn}}</p>
            <p class="intro">{{operation.intro}}</p>

            <div class="operating-list">
              <p class="text" v-for="(item,key) in operation.data" :key="key"
                 :class="{'active-text':key===operationBannerNow}" @click="slideToBanner(key)">
                {{item.name}}
                <span class="dot"></span>
              </p>
            </div>
          </div>
          <div class="float-left swiper-box">
            <div class="swiper-container" id="operationBanner">
              <div class="swiper-wrapper">
                <div class="swiper-slide" v-for="item in operation.data">
                  <div class="img-box">
                    <img :src="item.image" alt="">
                  </div>
                  <span class="dot" v-for="dot in item.dot" :style="'left:'+dot.left+'vw;top:'+dot.top+'vh'"><n-link
                    to="">{{dot.name}}</n-link></span>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="more-box">
          <div class="text-box">
            <div class="show-box">
              <n-link to="/agriculture">
                <p class="text text-en">MORE</p>
                <p class="text">了解更多</p>
              </n-link>
            </div>
          </div>
        </div>
      </section>


      <section class="bottom-box">
        <div class="fishBox wow bounceInLeft" data-wow-duration="2s">
        </div>
        <div class="content-box clearfix">
          <div class="float-left text-box">
            <p class="title1 animated wow fadeInUp" data-wow-duration=".4s" data-wow-delay="1.4s">
              {{companyInfo.title1}}</p>
            <p class="title2 animated wow fadeInUp" data-wow-duration=".4s" data-wow-delay="1.8s">
              {{companyInfo.title2}}</p>
            <p class="title3 animated wow fadeInUp" data-wow-duration=".4s" data-wow-delay="2.2s">
              {{companyInfo.title3}}</p>
            <p class="title4 animated wow fadeInUp" data-wow-duration=".4s" data-wow-delay="2.6s">
              {{companyInfo.title4}}</p>
            <p class="intro1 animated wow fadeInUp" data-wow-duration=".4s" data-wow-delay="3s">
              {{companyInfo.intro1}}</p>
            <p class="intro2 animated wow fadeInUp" data-wow-duration=".4s" data-wow-delay="3.4s">
              {{companyInfo.intro2}}</p>
            <div class="btn-box wow fadeInUp" data-wow-duration=".4s" data-wow-delay="3.8s">
              <n-link to="" class="btn-list" :class="{'btn-list-active':cooperationBtn}">
                <div @mouseover="changBtnStatus(true)">合作伙伴</div>
              </n-link>

              <n-link to="" class="btn-list" :class="{'btn-list-active':!cooperationBtn}">
                <div @mouseover="changBtnStatus(false)">资讯动态</div>
              </n-link>
            </div>
          </div>
          <div class="float-left number-box">
            <div class="container-fluid">
              <div class="row">
                <div class="col-4 wow fadeInUp" data-wow-duration=".4s" data-wow-delay=".2s">
                  <p>
                    <span class="number">{{growthNumberE1}}</span>
                    <span class="symbol">+</span>
                  </p>
                  <p class="name">服务企业总数</p>
                </div>
                <div class="col-4 wow fadeInUp" data-wow-duration=".4s" data-wow-delay=".4s">
                  <p>
                    <span class="number">{{growthNumberE2}}</span>
                    <span class="symbol">+</span>
                  </p>
                  <p class="name">服务大型企业</p>
                </div>
                <div class="col-4 wow fadeInUp" data-wow-duration=".4s" data-wow-delay=".6s">
                  <p>
                    <span class="number">{{growthNumberE3}}</span>
                    <span class="symbol">年</span>
                  </p>
                  <p class="name">专注旅游行业</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <Footer/>
    </div>
  </div>
</template>

<script>
  import Nav from "~/components/navigationBar.vue"
  import Footer from "~/components/footer.vue"
  import Shortcut from "~/components/rightBtn.vue"
  import Swiper from "swiper"
  import axios from "axios"
  import {TweenLite} from "gsap"
  import "swiper/css/swiper.min.css"
  import "swiper-animate-cn/animate.min.css"
  import {swiperAnimateCache, swiperAnimate, clearSwiperAnimate} from "swiper-animate-cn"

  export default {
    components: {
      Nav,
      Footer,
      Shortcut
    },
    data() {
      return {
        loadingStatus: false,
        homeBannerCurrent: "",
        operationBanner: "",
        operationBannerNow: "",
        bgLogoLocation: 0,
        bgLogoLocationRightStatus: false,
        bgLogoLocationLeftStatus: false,
        scrollTop: 0,
        cooperationBtn: true,
        growthNumber1: 0,//服务企业总数初始数字
        growthNumber2: 0,//服务大型企业初始数字
        growthNumber3: 0,//专注旅游行业初始数字
        growthNumberA1: 0,//服务企业总数动画数字
        growthNumberA2: 0,//服务大型企业动画数字
        growthNumberA3: 0,//专注旅游行业动画数字
        growthNumberH1: 0,//服务企业总数
        growthNumberH2: 0,//服务大型企业
        growthNumberH3: 0,//专注旅游行业
        homeBanner: "",
        tourismData: "",
        agriculture: "",
        operation: "",
        companyInfo: ""
      }
    },
    watch: {
      growthNumber1: function (value) {
        let v = this;
        TweenLite.to(v.$data, 1.5, {growthNumberA1: value});
      },
      growthNumber2: function (value) {
        let v = this;
        TweenLite.to(v.$data, 1.5, {growthNumberA2: value});
      },
      growthNumber3: function (value) {
        let v = this;
        TweenLite.to(v.$data, .8, {growthNumberA3: value});
      },

      loadingStatus: function () {
        this.$nextTick(() => {
          this.initHomeBanner();
          this.initOperationBanner();
          if (process.browser) {
            let {WOW} = require("wowjs");
            let wow = new WOW({
              live: false,
              mobile: false
            });
            wow.init();
          }
        });
      }
    },
    computed: {
      growthNumberE1: function () {
        return this.growthNumberA1.toFixed(0);
      },
      growthNumberE2: function () {
        return this.growthNumberA2.toFixed(0);
      },
      growthNumberE3: function () {
        return this.growthNumberA3.toFixed(0);
      }
    },
    mounted() {

      window.addEventListener('scroll', this.getScroll);
      this.getScroll();
    },
    created() {
      this.getData();
    },
    destroyed() {
      window.removeEventListener('scroll', this.getScroll);
    },
    methods: {
      // 获取页面数据
      getData: function () {
        let self = this;
        axios.get('/data/index.json')
          .then(res => {
            if (res.status === 200) {
              self.homeBanner = res.data.homeBanner;
              self.tourismData = res.data.tourismData;
              self.agriculture = res.data.agriculture;
              self.operation = res.data.operation;
              self.companyInfo = res.data.companyInfo;
              self.growthNumberH1 = res.data.growthNumberH1;
              self.growthNumberH2 = res.data.growthNumberH2;
              self.growthNumberH3 = res.data.growthNumberH3;

              self.loadingStatus = true;
            }
          })
      },
      // 滚动事件
      getScroll() {
        let el = document.querySelector(".number-box .row");
        let sectionH = el.clientHeight;//元素高度
        let top = el.getBoundingClientRect().top; //元素顶端到可见区域顶端的距离
        let se = document.documentElement.clientHeight;//浏览器可见区域高度。

        if (top + sectionH <= se) {
          this.setAnimatedNum()
        }
      },

      //设置动态数字
      setAnimatedNum() {
        setTimeout(() => {
          this.growthNumber1 = this.growthNumberH1;//服务企业总数
        }, 600);
        setTimeout(() => {
          this.growthNumber2 = this.growthNumberH2;//服务大型企业
        }, 800);
        setTimeout(() => {
          this.growthNumber3 = this.growthNumberH3;//专注旅游行业
        }, 1000);
      },
      // 改变图标状态
      changBtnStatus: function (status) {
        this.cooperationBtn = !!status;
      },
      // 初始化顶部banner
      initHomeBanner: function () {
        let v = this;
        let homeBanner = new Swiper('#homeBanner', {
          loop: true, // 循环模式选项
          speed: 800,
          autoplay: {
            delay: 3000,
            stopOnLastSlide: false,
            disableOnInteraction: true,
          },

          // 如果需要分页器
          pagination: {
            el: '.home-banner-pagination',
            clickable: true
          },

          on: {
            init: function () {
              let current = this.realIndex;
              v.homeBannerCurrent = current > 8 ? current + 1 : '0' + (current + 1);
              swiperAnimateCache(this); //隐藏动画元素
              swiperAnimate(this); //初始化完成开始动画
            },
            transitionStart: function () {
              let current = this.realIndex;
              v.homeBannerCurrent = current > 8 ? current + 1 : '0' + (current + 1);
            },
            slideChangeTransitionEnd: function () {
              swiperAnimate(this)
            }
          }
        })
      },

      // 初始化社会化运营banner
      initOperationBanner: function () {
        let v = this;
        v.operationBanner = new Swiper('#operationBanner', {
          loop: true, // 循环模式选项
          speed: 800,
          autoplay: {
            delay: 5000,
            stopOnLastSlide: false,
            disableOnInteraction: true,
          },
          on: {
            init: function () {
              v.operationBannerNow = this.realIndex;
            },
            transitionStart: function () {
              v.operationBannerNow = this.realIndex;
            }
          }
        })
      },

      // 切换到指定banner
      slideToBanner: function (index) {
        let v = this;
        v.operationBanner.slideToLoop(index);
      }
    }
  }
</script>

<style src="~/assets/css/index.css" scoped></style>
<style>
  .home-banner .home-banner-pagination {
    width: 12px;
    left: calc(2.05vw - 6px);
    top: 50%;
    transform: translateY(-50%);
  }

  .home-banner .swiper-pagination-bullet {
    width: 12px;
    height: 12px;
    background: none;
    border: 1px solid #aaaaaa;
    opacity: 1;
    transition: .3s;
  }

  .home-banner .swiper-pagination-bullet:focus {
    outline: none;
  }

  .home-banner .swiper-pagination-bullet-active {
    background: #c70b17;
    border-color: #c70b17;
  }

  .banner-box .number {
    font-size: 64px;
    color: #c8262b;
  }
  @media screen and (max-width: 1200px) {
    .banner-box .number {
      font-size: 26px;
    }
  }
</style>
