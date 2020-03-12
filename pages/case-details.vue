<template>
  <div>
    <Nav :isAbout="true"/>
    <Shortcut/>

    <section class="top-box">
      <div class="content-box">
        <p class="logo-box">
          <img :src="caseData.logo | baseDecodeImg1" alt="">
        </p>
        <div class="main-box" :style="caseData.img_url | baseDecodeImg">
        </div>
      </div>

      <div class="details-box" :class="{'details-box-position':!scrollTop}">
        <p class="name">{{caseData.title | baseDecode}}</p>
        <p class="name-en">{{caseData.sub_title | baseDecode}}</p>
        <p class="intro">{{caseData.zhaiyao | baseDecode}}</p>
        <p class="url">{{caseData.link_url}}</p>
        <p class="clearfix">
          <a v-if="caseData.link_url" :href="caseData.link_url" target="_blank" class="float-left url-link"></a>
<!--          <n-link to="" class="float-right next-link">NEXT</n-link>-->
        </p>
      </div>
    </section>

    <section class="bottom-details-box">
      <div class="content-box" v-html="baseDecodeHtml(caseData.content)"></div>
    </section>
    <Footer/>
  </div>
</template>
<script>
  import Nav from '~/components/navigationBar.vue'
  import Footer from '~/components/footer.vue'
  import Shortcut from '~/components/rightBtn.vue'
  import axios from "axios";
  import {Base64} from "js-base64"
  import VueCookies from "vue-cookies"

  export default {
    components: {
      Nav,
      Footer,
      Shortcut
    },
    data(){
      return{
        loadingStatus:"",
        caseData:{},
        scrollTop:true,
      }
    },
    created() {
      this.getData();
    },
    methods:{
      // 获取页面数据
      getData: function () {
        let self = this;

        console.log(self.caseData)
        self.caseData = VueCookies.get("article");
        self.caseData.link_url = Base64.decode(self.caseData.link_url);
        console.log(self.caseData.link_url)
        // axios.get('/data/case.json')
        //   .then(res => {
        //     if (res.status === 200) {
        //       self.bannerData = res.data.bannerData;
        //       self.agricultureData = res.data.agricultureData;
        //       self.caseData = res.data.caseData[0];
        //
        //       self.loadingStatus = true;
        //     }
        //   })
      },


      handleScroll() {
        let self = this;
        let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
        let screenHeight = document.documentElement.clientHeight;
        self.scrollTop = scrollTop > screenHeight/2;
      },

      baseDecodeHtml(e){
        return Base64.decode(e);
      }
    },
    mounted() {
      window.addEventListener("scroll", this.handleScroll) || document.addEventListener("scroll", this.handleScroll);
      this.handleScroll();
    },
    destroyed() {
      window.removeEventListener('scroll', this.handleScroll) || document.removeEventListener('scroll', this.handleScroll)
    },
    filters: {
      baseDecode: function (e) {
        return Base64.decode(e);
      },
      baseDecodeImg: function (e) {
        return "background-image:url(http://www.sunlue.com" + Base64.decode(e)+")";
      },
      baseDecodeImg1:function(){
        return "http://www.sunlue.com" + Base64.decode(e);
      }
    }
  }
</script>
<style scoped src="~/assets/css/case-details.css"></style>
<style>
  .bottom-details-box *{
    max-width: 100%;
  }
</style>
