<template>
  <div>
    <Nav :isWhite="true"/>
    <Shortcut/>
    <section class="case-box">
      <div class="title-box">
        <p class="title">合作伙伴</p>
        <p class="title-en">partners</p>
      </div>
      <ul class="list-inline nav-box">
        <li class="list-inline-item list" @click="switchNav(0)" :class="{'active':activeNum===0}">全部</li>
        <li class="list-inline-item list" @click="switchNav(1)" :class="{'active':activeNum===1}">全域旅游</li>
        <li class="list-inline-item list" @click="switchNav(2)" :class="{'active':activeNum===2}">大数据平台</li>
        <li class="list-inline-item list" @click="switchNav(3)" :class="{'active':activeNum===3}">数字乡村</li>
        <li class="list-inline-item list" @click="switchNav(4)" :class="{'active':activeNum===4}">数字服务中心</li>
      </ul>
      <div class="container-fluid main-box clearfix">
        <div class="item-list float-left" v-for="item in nowData">
          <div class="img-box">
            <i @click="setCookie(item)">
              <img :src="item.img_url | baseDecode" alt="">
            </i>
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
  import axios from "axios";
  import $ from "jquery";
  import {Base64} from "js-base64"
  import VueCookies from "vue-cookies"

  export default {
    components: {
      Nav,
      Footer,
      Shortcut
    },
    data() {
      return {
        loadingStatus: false,
        activeNum: 0,
        allData: "",
        nowData: [],
        requestData: "",
        page: 1,
        url: "",
        isLoading: false
      }
    },
    created() {
      this.getData(this.nowData);
    },
    watch: {
      /**
       *监听是否加载完成，加载完成执行
       */
      loadingStatus: function () {

      }
    },
    methods: {
      // 写入cookie
      setCookie:function(data){
        VueCookies.set("article",data);
        this.$router.push('/case-details')
      },
      // 获取页面数据
      getData: function (person) {
        let self = this;

        $.ajax({
          type: "POST",
          url: "http://www.sunlue.com/tools/Internal_ajax.ashx?action=getPubFlow",
          data: {
            "channel": "partner",
            "categoryId": "87",
            "pagesize": "15",
            "pageIndex": self.page,
            "where": "",
            "orderby": ""
          },
          dataType: "text",
          success: function (e) {
            self.page++;
            if (self.JStrToJArr(e).length > 0) {
              for (let i in self.JStrToJArr(e)) {
                person.push(self.JStrToJArr(e)[i]);
              }
              self.isLoading = false
            }
          },
          error: function (msg) {
            console.log("ajax_error");
          }
        });
      },

      //切换分类
      switchNav: function (num) {
        this.activeNum = num;
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

      // 滑动加载事件
      scroll(person) {
        let self = this;
        window.onscroll = () => {
          // 距离底部200px时加载一次
          let bottomOfWindow = document.documentElement.offsetHeight - document.documentElement.scrollTop - window.innerHeight <= 200
          if (bottomOfWindow && self.isLoading === false) {
            self.isLoading = true;
            self.getData(person);
          }
        }
      }
    },
    mounted() {
      this.scroll(this.nowData)
    },
    filters: {
      baseDecode: function (e) {
        return "http://www.sunlue.com" + Base64.decode(e);
      }
    }
  }
</script>
<style scoped src="~/assets/css/case.css"></style>
