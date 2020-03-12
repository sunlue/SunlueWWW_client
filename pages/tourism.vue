<template>
  <div>
    <div class="tourism">
      <Nav />
      <Shortcut/>

      <section class="home-banner tourism-banner">
        <div class="banner-box">
          <div class="swiper-container" id="homeBanner">
            <div class="swiper-wrapper">
              <div class="swiper-slide" v-for="item in homeBanner">
                <div class="img-box">
                  <img :src="item.url" alt="">
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

      <section class="scenic-box">
        <div class="content-box">
          <div class="text-box">
            <p class="name">{{scenicData.name}}</p>
            <p class="name-en" v-html="scenicData.nameEn"></p>
            <p class="intro" v-html="scenicData.intro"></p>
            <p class="intro1">{{scenicData.intro1}}</p>
          </div>
          <div class="list-box">
            <div class="container-fluid">
              <div class="row">
                <div class="col" v-for="item in scenicData.data">
                  <div class="img-box">
                    <img :src="item.img" alt="">
                  </div>
                  <p class="name">
                    {{item.name}}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="hotel-box">
        <div class="content-box">
          <div class="text-box">
            <p class="name">{{hotelData.name}}</p>
            <p class="name-en" v-html="hotelData.nameEn"></p>
            <p class="intro" v-html="hotelData.intro"></p>
            <p class="intro1">{{hotelData.intro1}}</p>
          </div>
          <div class="list-box">
            <div class="container-fluid">
              <div class="row">
                <div class="col" v-for="item in hotelData.data">
                  <div class="img-box">
                    <img :src="item.img" alt="">
                  </div>
                  <p class="name">
                    {{item.name}}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="travel-box">
        <div class="content-box clearfix">
          <div class="float-left left-box">
            <img class="img-fluid" :src="travelData.img1" alt="">
          </div>
          <div class="float-right right-box">
            <p class="title">{{travelData.title}}</p>
            <p class="titleEn" v-html="travelData.titleEn"></p>
            <p class="title2" v-html="travelData.title2"></p>
            <p class="intro">{{travelData.intro}}</p>
            <p class="intro">{{travelData.intro1}}</p>
            <div class="img-box">
              <img class="img-fluid" :src="travelData.img2" alt="">
              <img class="img-fluid" :src="travelData.img3" alt="">
            </div>
          </div>
        </div>
      </section>

      <section class="internet-box">
        <div class="content-box">
          <div class="title-box">
            <p class="title">{{internetData.title}}</p>
            <p class="titleEn" v-html="internetData.titleEn"></p>
            <p class="intro">{{internetData.intro}}</p>
          </div>
          <div class="container-fluid">
            <div class="row">
              <div class="col-lg-3 list-box" v-for="item in internetData.data">
                <div class="border-box">
                  <div class="img-box">
                    <img :src="item.img" alt="">
                  </div>
                  <p class="title">{{item.title}}</p>
                  <p class="intro">{{item.intro}}</p>
                  <p class="type">{{item.type}}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="interactive-box">
        <div class="content-box">
          <div class="title-box">
            <p class="title">{{interactiveData.title}}</p>
            <p class="titleEn" v-html="interactiveData.titleEn"></p>
            <p class="intro">{{interactiveData.intro}}</p>
          </div>

          <div class="container-fluid group-box">
            <div class="row">
              <div class="col-lg-4 list-box" v-for="item in interactiveData.data">
                <div class="float-left bg-box">
                </div>
                <div class="float-left text-box">
                  <p class="title">{{item.title}}</p>
                  <p class="intro" v-for="intro in item.intro">{{intro}}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="partner-box">
        <div class="content-box">
          <div class="title-box">
            <p class="title">{{partnerData.title}}</p>
            <p class="titleEn" v-html="partnerData.titleEn"></p>
          </div>
          <div class="list-box">
            <div class="row">
              <div class="col-lg-3 col-6 list-item" v-for="item in partnerData.data">
                <div class="img-box">
                  <img :src="item.img" alt="">
                </div>
              </div>
            </div>
          </div>
          <p class="more-btn">
            <n-link to="/case" class="more-link">更多</n-link>
          </p>
        </div>
      </section>
      <Footer />
    </div>
  </div>
</template>

<script>
  import Nav from '~/components/navigationBar.vue'
  import Footer from '~/components/footer.vue'
  import Shortcut from "~/components/rightBtn.vue"
  import Swiper from "swiper";
  import {swiperAnimate, swiperAnimateCache} from "swiper-animate-cn";
  import axios from "axios";

  export default {
    components: {
      Nav,
      Footer,
      Shortcut
    },
    data(){
      return{
        loadingStatus:false,
        homeBannerCurrent: "",
        homeBanner: "",

        scenicData:"",

        hotelData:"",

        travelData:"",

        internetData:"",

        interactiveData:"",

        partnerData:""
      }
    },
    mounted() {

    },
    created(){
      this.getData();
    },
    watch:{
      loadingStatus: function () {
        this.$nextTick(()=>{
          this.initHomeBanner();
        })
      }
    },
    methods:{
      // 获取页面数据
      getData: function () {
        let self = this;
        axios.get('/data/tourism.json')
          .then(res => {
            if (res.status === 200) {
              self.homeBanner = res.data.homeBanner;
              self.scenicData = res.data.scenicData;
              self.hotelData = res.data.hotelData;
              self.travelData = res.data.travelData;
              self.internetData = res.data.internetData;
              self.interactiveData = res.data.interactiveData;
              self.partnerData = res.data.partnerData;

              self.loadingStatus = true;
            }
          })
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
    }
  }
</script>
<style src="~/assets/css/tourism.css" scoped></style>
<style>
  .tourism-banner .swiper-pagination-bullet {
    border: 1px solid #c8262b !important;
  }

  .tourism .red-text{
    color: #c8262b;
  }

</style>
