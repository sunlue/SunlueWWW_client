<template>
  <div>
    <Nav :isWhite="true"/>
    <Shortcut/>

    <section class="operation-box">
      <div class="swiper-container" id="operationBanner">
        <div class="swiper-wrapper">
          <div class="swiper-slide" v-for="item in operationBanner">
            <div class="img-box">
              <img :src="item" alt="">
            </div>
          </div>
        </div>
        <!-- 如果需要导航按钮 -->
        <div class="swiper-button-prev operation-banner-prev">PREV</div>
        <div class="swiper-button-next operation-banner-next">NEXT</div>

        <div class="swiper-pagination operation-banner-pagination"></div>
      </div>
      <div class="home-banner-total">
        <span class="current">{{homeBannerCurrent}}</span>
        <span class="separator">/</span>
        <span class="total">{{operationBanner.length>10?operationBanner.length:'0'+operationBanner.length}}</span>
      </div>
      <div class="operation-banner-text">
        <p class="p1 wow fadeInUp">乡村振兴</p>
        <p class="p2 wow fadeInUp" data-wow-delay=".4s">服务中心运营</p>
        <p class="p3 wow fadeInUp" data-wow-delay=".8s">Rural revitalization</p>
        <p class="p4 wow fadeInUp" data-wow-delay="1.2s">Service center operation</p>
        <div class="round"></div>
      </div>
    </section>

    <section class="operating-box">
      <div class="clearfix">
        <div class="float-left text-box">
          <div class="title-box">
            <p class="p1">{{operatingTitle.title}}</p>
            <p class="p2" v-html="operatingTitle.titleEn"></p>
            <p class="p3">{{operatingTitle.intro}}</p>
            <p class="p4">{{operatingTitle.intro1}}</p>
          </div>

          <div class="list-box">
            <div class="list-item" v-for="(item,key) in operatingData" :class="{'active-list':operatingActiveNumber===key}" @click="checkIndex(key)">
              <p class="name">{{item.name}}</p>
              <p class="name-en">{{item.nameEn}}</p>
            </div>
          </div>
        </div>
        <div class="float-right content-box">
          <div class="swiper-box float-right">
            <div class="swiper-container" id="operatingData">
              <div class="swiper-wrapper">
                <div class="swiper-slide" v-for="item in operatingData">
                  <div class="img-box">
                    <img :src="item.image" alt="">
                  </div>
                </div>
              </div>

            </div>
          </div>
        </div>
      </div>

      <div class="detailsBox">
        <p class="name">{{operatingActiveData.name}}</p>
        <p class="intro">{{operatingActiveData.intro}}</p>

        <div class="btn-box">
          <div class="swiper-button-prev operating-data-prev" @click="checkPrev()"></div>
          <div class="swiper-button-next operating-data-next" @click="checkNext()"></div>
        </div>
      </div>
    </section>
    <Footer/>
  </div>
</template>

<script>
  import Nav from '~/components/navigationBar.vue'
  import Footer from '~/components/footer.vue'
  import Shortcut from '~/components/rightBtn.vue'
  import Swiper from 'swiper'
  import "swiper/css/swiper.min.css"
  import "swiper-animate-cn/animate.min.css"
  import axios from "axios";

  export default {
    components: {
      Nav,
      Footer,
      Shortcut
    },
    data() {
      return {
        loadingStatus:false,
        homeBannerCurrent: "",
        operationBanner: "",
        operatingTitle: "",
        operatingActiveData: "",
        operatingActiveNumber:0,
        operatingSwiper: "",
        operatingData: ""
      }
    },
    watch:{
      loadingStatus:function(){
        this.$nextTick(() => {
          this.initBanner();
          this.initDataSwiper();
          if (process.browser) {
            let {WOW} = require("wowjs");
            let wow = new WOW({
              live: false,
              mobile:false
            });
            wow.init();
          }
        })
      }
    },
    mounted() {
    },
    created() {
      this.getData();
    },
    methods: {
      // 获取页面数据
      getData: function () {
        let self = this;
        axios.get('/data/operation.json')
          .then(res => {
            if (res.status === 200) {
              self.operationBanner = res.data.operationBanner;
              self.operatingTitle = res.data.operatingTitle;
              self.operatingData = res.data.operatingData;

              self.loadingStatus = true;
            }
          })
      },

      //初始化banner
      initBanner: function () {
        let v = this;
        new Swiper('#operationBanner', {
          loop: true, // 循环模式选项
          speed: 800,
          autoplay: {
            delay: 5000,
            stopOnLastSlide: false,
            disableOnInteraction: true,
          },
          // 如果需要前进后退按钮
          navigation: {
            nextEl: '.operation-banner-next',
            prevEl: '.operation-banner-prev',
          },
          // 如果需要分页器
          pagination: {
            el: '.operation-banner-pagination',
          },
          on: {
            init: function () {
              let current = this.realIndex;
              v.homeBannerCurrent = current > 8 ? current + 1 : '0' + (current + 1);
            },
            transitionStart: function () {
              let current = this.realIndex;
              v.homeBannerCurrent = current > 8 ? current + 1 : '0' + (current + 1);
            },
          }
        })
      },

      // 初始化数据轮播
      initDataSwiper: function () {
        let v = this;
        v.operatingSwiper = new Swiper('#operatingData', {
          loop: true, // 循环模式选项
          speed: 800,
          autoplay: {
            delay: 3000,
            stopOnLastSlide: false,
            disableOnInteraction: true,
          },
          effect: "fade",

          on: {
            init: function () {
              let index = this.realIndex;
              v.operatingActiveData = v.operatingData[index];
              v.operatingActiveNumber = index;
            },
            transitionStart: function () {
              let index = this.realIndex;
              v.operatingActiveData = v.operatingData[index];
              v.operatingActiveNumber = index;
              this.navigation.update();
            }
          }
        })
      },

      checkIndex(index) {
        let v = this;
        v.operatingSwiper.slideToLoop(index);
      },
      checkPrev(){
        let v = this;
        v.operatingSwiper.slidePrev();
      },
      checkNext(){
        let v = this;
        v.operatingSwiper.slideNext();
      },
    }
  }
</script>
<style scoped src="~/assets/css/operation.css"></style>

<style>
  .operation-banner-pagination .swiper-pagination-bullet {
    width: 12px;
    height: 12px;
    margin: 0 15px !important;
    background: none;
    opacity: 1;
    border: 2px solid #aaa;
  }

  .operation-banner-pagination .swiper-pagination-bullet-active {
    border: none;
    background: #c70b17;
  }

  .operating-box .red-text {
    color: #c70b17;
  }
</style>

