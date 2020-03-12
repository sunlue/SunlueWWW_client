<template>
  <div>
    <div class="bg-box about-box">
      <Nav :isAbout="true"/>
      <Shortcut/>
      <section class="top-box">
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

      <section class="service-box">
        <div class="content-box">
          <div class="title-box">
            <p class="title1">{{serviceData.title1}}</p>
            <p class="title2">{{serviceData.title2}}</p>
            <p class="title3" v-html="serviceData.title3"></p>
          </div>
          <div class="row list-item-box">
            <div class="col animated wow fadeInUp" :data-wow-delay="key*.2+'s'" v-for="(list,key) in serviceData.data">
              <div class="img-box">
                <img :src="list.img" alt="">
              </div>
              <p class="title">{{list.title}}</p>
              <ul class="item-box">
                <li v-for="item in list.item" class="item-title">
                  {{item.title}}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </section>

      <section class="partners-box">
        <div class="content-box">
          <div class="title-box">
            <p v-html="partnersData.title"></p>
          </div>
          <div class="clearfix">
            <div class="text-box float-left">
              <p class="name">
                <span class="name-cn">{{partnersActiveData.name}}</span>
                <span class="name-en">{{partnersActiveData.nameEn}}</span>
              </p>
              <p class="intro">{{partnersActiveData.intro}}</p>
              <p class="link">
                <a :href="partnersActiveData.src?partnersActiveData.src:'javascript:void(0)'"
                   :target="partnersActiveData.src?'_blank':'_self'">{{partnersActiveData.src}}</a>
              </p>
              <p class="text-more">
                <n-link to="/case">了解更多</n-link>
              </p>
            </div>
            <div class="swiper-box float-right">
              <div class="swiper-container" id="partners-swiper">
                <div class="swiper-wrapper">
                  <div class="swiper-slide" v-for="item in partnersData.item">
                    <div class="img-box">
                      <img :src="item.image" alt="">
                    </div>
                  </div>
                </div>

                <!-- 如果需要分页器 -->
                <div class="swiper-pagination partners-pagination"></div>
              </div>

              <div class="button-box">
                <!-- 如果需要导航按钮 -->
                <div class="swiper-button-prev partners-button-prev"></div>
                <div class="swiper-button-next partners-button-next"></div>
              </div>
            </div>
          </div>

          <n-link to="/case" class="more">More</n-link>
        </div>
      </section>

      <section class="news-box">
        <div class="content-box">
          <div class="title-box">
            <p v-html="newsData.title"></p>
          </div>

          <div class="container-fluid list-box ">
            <div class="row">
              <div class="col-lg-6 item-box fist-box"  @click="showModal(item)" v-if="index<1" v-for="(item,index) in newsData">
                <div class="img-box">
                  <img :src="item.img_url | baseDecodeImg" alt="">
                  <p class="clearfix name-box">
                    <span class="float-left">{{item.title | baseDecode}}</span>
                    <span class="float-right">{{item.update_time | baseDecode}}</span>
                  </p>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="row">
                  <div class="col-lg-12 item-box"  @click="showModal(item)" v-if="index<3&index>0" v-for="(item,index) in newsData">
                    <div class="border-box clearfix" :class="{'margin0':index==2}">
                      <div class="float-left time-box">
                        <p class="time-md">{{item.update_time | formatDateMD}}</p>
                        <p class="time-y">{{item.update_time | formatDateY}}</p>
                      </div>
                      <div class="float-left details-box">
                        <p class="name">{{item.title | baseDecode}}</p>
                        <p class="intro">{{item.zhaiyao | baseDecode}}</p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="col-lg-6 item-box"  @click="showModal(item)" v-if="index>=3" v-for="(item,index) in newsData">
                <div class="border-box clearfix margin-top-10">
                  <div class="float-left time-box">
                    <p class="time-md">{{item.update_time | formatDateMD}}</p>
                    <p class="time-y">{{item.update_time | formatDateY}}</p>
                  </div>
                  <div class="float-left details-box">
                    <p class="name">{{item.title | baseDecode}}</p>
                    <p class="intro">{{item.zhaiyao | baseDecode}}</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <i @click="getNews()" class="more">More</i>
        </div>
      </section>
      <b-modal id="modal-xl" centered hide-footer size="xl" :title="activeArticle.title | baseDecode" >
        <div v-html="baseDecodeHtml(activeArticle.content)">
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. Architecto assumenda dolore ex expedita facilis perferendis quam repudiandae. Ad aliquam asperiores corporis deleniti eaque incidunt iure, odio officiis perferendis vero voluptates?
        </div>
      </b-modal>
      <Footer/>
    </div>
  </div>
</template>

<script>
  import Nav from '~/components/navigationBar.vue'
  import Footer from '~/components/footer.vue'
  import Shortcut from '~/components/rightBtn.vue'
  import Swiper from 'swiper'
  import {TweenLite} from "gsap"
  import "swiper/css/swiper.min.css"
  import "swiper-animate-cn/animate.min.css"
  import axios from "axios";
  import $ from "jquery";
  import {Base64} from "js-base64"

  export default {
    components: {
      Nav,
      Footer,
      Shortcut
    },
    data() {
      return {
        loadingStatus: false,
        growthNumber1: 0,//服务企业总数初始数字
        growthNumber2: 0,//服务大型企业初始数字
        growthNumber3: 0,//专注旅游行业初始数字
        growthNumberA1: 0,//服务企业总数动画数字
        growthNumberA2: 0,//服务大型企业动画数字
        growthNumberA3: 0,//专注旅游行业动画数字
        growthNumberH1: 0,//服务企业总数
        growthNumberH2: 0,//服务大型企业
        growthNumberH3: 0,//专注旅游行业
        companyInfo: "",
        serviceData: "",
        partnersActiveData: "",
        partnersData: "",
        newsData: [],
        page: 1,
        isLoading: false,
        activeArticle:{
          title:"",
          content:""
        }
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
          if (process.browser) {
            let {WOW} = require("wowjs");
            let wow = new WOW({
              live: false,
              mobile: false
            });
            wow.init();
          }

          this.initPartnersSwiper();

          this.setAnimatedNum();
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

    },
    created() {
      this.getData();
      this.getNews();
    },

    methods: {
      showModal(val) {
        let self = this;
        self.activeArticle = val;
        this.$bvModal.show("modal-xl");
      },
      // 获取页面数据
      getData: function () {
        let self = this;
        axios.get('/data/about.json')
          .then(res => {
            if (res.status === 200) {
              self.companyInfo = res.data.companyInfo;
              self.serviceData = res.data.serviceData;
              self.partnersData = res.data.partnersData;
              // self.newsData = res.data.newsData;

              self.growthNumberH1 = res.data.growthNumberH1;
              self.growthNumberH2 = res.data.growthNumberH2;
              self.growthNumberH3 = res.data.growthNumberH3;

              self.loadingStatus = true;
            }
          })
      },

      getNews: function () {
        let self = this;
        let person = self.newsData;
        $.ajax({
          type: "POST",
          url: "http://www.sunlue.com/tools/Internal_ajax.ashx?action=getPubFlow",
          data: {
            "channel": "news",
            "categoryId": "86",
            "pagesize": "5",
            "pageIndex": self.page,
            "where": "",
            "orderby": "",
          },
          dataType: "text",
          success: function (e) {
            self.page++;
            if (self.JStrToJArr(e).length > 0) {
              for (let i in self.JStrToJArr(e)) {
                person.push(self.JStrToJArr(e)[i]);
              }
              self.isLoading = false
              self.$forceUpdate()
            }
          },
          error: function (msg) {
            console.log("ajax_error");
          }
        });
      },

      JStrToJArr: function (string) {
        if (string == "[]") return "";
        let datearr = string.split("},");
        $.each(datearr, function (i, e) {
          datearr[i] = datearr[i].replace("[", "").replace("]", "");
          if (datearr[i][datearr[i].length - 1] != "}") datearr[i] = datearr[i] + "}";
          let json = eval('(' + datearr[i] + ')');
          datearr[i] = json;
        });
        return datearr;
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

      // 初始化合作伙伴
      initPartnersSwiper() {
        let v = this;
        new Swiper('#partners-swiper', {
          loop: true,
          speed: 800,

          // 如果需要分页器
          pagination: {
            el: '.swiper-pagination',
            clickable: true
          },

          // 如果需要前进后退按钮
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
          },

          on: {
            init: function () {
              let index = this.realIndex;
              v.partnersActiveData = v.partnersData.item[index];
            },
            transitionStart: function () {
              let index = this.realIndex;
              v.partnersActiveData = v.partnersData.item[index];
            }
          }

        })
      },

      baseDecodeHtml(e){
        return Base64.decode(e);
      }
    },
    filters: {
      formatDateY: function (val) {
        let value = Base64.decode(val);
        let date = new Date(value.replace(/-/g, '/'));
        let y = date.getFullYear();
        return y;
      },
      formatDateMD: function (val) {
        let value = Base64.decode(val);
        let date = new Date(value.replace(/-/g, '/'));
        let MM = date.getMonth() + 1;
        MM = MM < 10 ? ('0' + MM) : MM;
        let d = date.getDate();
        d = d < 10 ? ('0' + d) : d;
        return MM + '-' + d;
      },
      baseDecode: function (e) {
        return Base64.decode(e);
      },
      baseDecodeImg: function (e) {
        return "http://www.sunlue.com" + Base64.decode(e);
      }
    }
  }
</script>
<style src="~/assets/css/about.css" scoped></style>
<style>
  .about-box .red-text {
    color: #c8262b;
  }

  #partners-swiper .swiper-pagination-bullet {
    opacity: 1;
    width: 12px;
    height: 12px;
    margin: 0 10px;
    border: 1px solid #aaa;
    background: none;
  }

  #partners-swiper .swiper-pagination-bullet-active {
    background: #c8262b;
    border-color: #c8262b;
  }

  #modal-xl .modal-header h5{
    color: #c8262b;
    font-size: 36px;
  }
</style>
