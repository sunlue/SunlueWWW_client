<template>
  <div>
    <Nav :isWhite="true"/>
    <Shortcut :isGreen="true"/>
    <section class="agriculture-banner-box">
      <AniBg/>
      <div class="title-box">
        <div>
          <div class="title">
            <span>智</span>
            <span>慧</span>
            <span>农</span>
            <span>业</span>
          </div>
          <p class="title-en">{{bannerData.nameEn}}</p>
        </div>
        <div>
          <p class="intro">{{bannerData.intro}}</p>
        </div>
      </div>
      <div class="bottom-box">
        <div class="list-box">
          <div class="icon-box">
            <div class="icon-bg"></div>
          </div>
          <p class="name">
            乡村振兴农业农村
          </p>
        </div>
        <div class="list-box">
          <div class="icon-box">
            <div class="icon-bg"></div>
          </div>
          <p class="name">
            数字乡村
          </p>
        </div>
        <div class="list-box">
          <div class="icon-box">
            <div class="icon-bg"></div>
          </div>
          <p class="name">
            农业农村大数据
          </p>
        </div>
        <div class="list-box">
          <div class="icon-box">
            <div class="icon-bg"></div>
          </div>
          <p class="name">
            农业大数据
          </p>
        </div>
      </div>
    </section>

    <section class="agriculture-content-box">
      <div class="content-box clearfix">
        <div class="left-box float-left">
          <div class="box">
            <div class="btn-list-box clearfix">
              <div class="float-left btn-box" @click="checkIndex(0)"
                   :class="{'active-box':agricultureActiveNumber===0}">
                <div class="icon-bg"><span class="icon-img"></span></div>
                <div class="name">乡村振兴农业农村</div>
              </div>
              <div class="float-left btn-box" @click="checkIndex(1)"
                   :class="{'active-box':agricultureActiveNumber===1}">
                <div class="icon-bg"><span class="icon-img"></span></div>
                <div class="name">农业大数据</div>
              </div>
              <div class="float-left btn-box" @click="checkIndex(2)"
                   :class="{'active-box':agricultureActiveNumber===2}">
                <div class="icon-bg"><span class="icon-img"></span></div>
                <div class="name">数字乡村</div>
              </div>
              <div class="float-left btn-box" @click="checkIndex(3)"
                   :class="{'active-box':agricultureActiveNumber===3}">
                <div class="icon-bg"><span class="icon-img"></span></div>
                <div class="name">农业农村大数据</div>
              </div>
            </div>

            <div class="swiper-container" id="agricultureData">
              <div class="swiper-wrapper">
                <div class="swiper-slide list-item" v-for="item in agricultureData">
                  <div class="img-box">
                    <img :src="item.image" alt="">
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="right-box ">
          <p class="name">{{agricultureActiveData.name}}</p>
          <p class="name-en">{{agricultureActiveData.nameEn}}</p>
          <p class="parting"></p>
          <p class="title">{{agricultureActiveData.title}}</p>
          <p class="intro">{{agricultureActiveData.intro}}</p>

          <div class="swiper-check-btn">
            <div class="swiper-button-prev agriculture-data-prev"></div>
            <div class="swiper-button-next agriculture-data-next"></div>
          </div>
        </div>
      </div>
    </section>

    <section class="agriculture-show">
      <div class="content-box">
        <p class="title">
          项目展示
        </p>
        <div class="clearfix">
          <div class="computer-box float-left">
            <img class="img-fluid computer-img" src="../assets/images/computerBg.png" alt="">
            <div class="swiper-box">
              <div class="swiper-container" id="caseShow">
                <div class="swiper-wrapper">
                  <div class="swiper-slide" v-for="item in caseData">
                    <div class="img-box">
                      <img :src="item.image" alt="">
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <n-link to="/case" class="more">
              了解更多
            </n-link>
          </div>
          <div class="rolling-text-box float-lg-right">
            <div class="list-box" v-for="(item,key) in caseData" @click="checkCaseIndex(key)" :class="{'active-box':caseActiveNumber===key}">
              <p class="name">{{item.name}}</p>
              <p class="nameEn">{{item.nameEn}}</p>
            </div>
          </div>
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
  import AniBg from '~/components/animationTextBg.vue'
  import Swiper from 'swiper'
  import "swiper/css/swiper.min.css"
  import axios from "axios";

  export default {
    components: {
      Nav,
      Footer,
      Shortcut,
      AniBg
    },
    mounted() {
    },
    created() {
      this.getData();
    },
    watch:{
      loadingStatus:function(){
        this.$nextTick(() => {
          this.initAgriculture();
          this.initCaseShow();
        });
      }
    },
    data() {
      return {
        loadingStatus:"",
        bannerData: "",
        agricultureActiveData: {},
        agricultureActiveSwiper: "",
        caseActiveSwiper: "",
        agricultureActiveNumber: 0,
        caseActiveNumber: 0,
        agricultureData: "",
        caseData: ""
      }
    },
    methods: {
      // 获取页面数据
      getData: function () {
        let self = this;
        axios.get('/data/agriculture.json')
          .then(res => {
            if (res.status === 200) {
              self.bannerData = res.data.bannerData;
              self.agricultureData = res.data.agricultureData;
              self.caseData = res.data.caseData;

              self.loadingStatus = true;
            }
          })
      },


      checkIndex(index) {
        let v = this;
        v.agricultureActiveSwiper.slideToLoop(index);
      },

      checkCaseIndex(index) {
        let v = this;
        v.caseActiveSwiper.slideToLoop(index);
      },

      // 初始化轮播图
      initAgriculture() {
        let v = this;
        v.agricultureActiveSwiper = new Swiper('#agricultureData', {
          observer: true,
          observeParents: true,
          speed: 800,
          spaceBetween: -80,
          centeredSlides: true,
          slidesPerView: 1,
          watchSlidesProgress: true,
          initialSlide: 1,
          loop: true,
          navigation: {
            nextEl: '.agriculture-data-next',
            prevEl: '.agriculture-data-prev',
          },
          breakpoints:{
            1200:{
              slidesPerView:1.1
            }
          },
          on: {
            setTranslate: function () {
              let slides = this.slides
              for (let i = 0; i < slides.length; i++) {
                let slide = slides.eq(i)
                let progress = slides[i].progress
                slide.transform('scale(' + (1 - Math.abs(progress) / 8) + ')');
              }
            },
            setTransition: function (transition) {
              for (let i = 0; i < this.slides.length; i++) {
                let slide = this.slides.eq(i)
                slide.transition(transition);
              }
            },
            init: function () {
              let index = this.realIndex;
              v.agricultureActiveData = v.agricultureData[index];
              v.agricultureActiveNumber = index;
            },
            transitionStart: function () {
              let index = this.realIndex;
              v.agricultureActiveData = v.agricultureData[index];
              v.agricultureActiveNumber = index;
            }
          }
        })
      },

      //初始化项目案例
      initCaseShow:function(){
        let v = this;
        v.caseActiveSwiper = new Swiper ('#caseShow', {
          loop: true,
          speed: 600,
          autoplay:true,
          effect : 'fade',
          on:{
            init:function(){
              let index = this.realIndex;
              v.caseActiveNumber = index
            },
            transitionStart:function () {
              let index = this.realIndex;
              v.caseActiveNumber = index
            }
          }
        })
      }
    }
  }
</script>
<style scoped src="~/assets/css/agriculture.css"></style>
<style>
  #agricultureData .swiper-slide-next {
    transition: .3s;
    opacity: 0;
  }

  #agricultureData .swiper-slide-prev {
    transition: .3s;
  }

  #agricultureData .swiper-slide-prev .img-box {
    position: relative;
  }

  #agricultureData .swiper-slide-prev .img-box::after {
    position: absolute;
    content: '';
    width: 100%;
    height: 100%;
    background: rgba(255, 255, 255, 0.6);
    left: 0;
    top: 0;
  }

  #agricultureData .swiper-slide-active .img-box {
    transform: translateX(30px);
  }

  #agricultureData .swiper-slide-prev .img-box {
    transform: translate(93%, 39px);
  }
</style>
