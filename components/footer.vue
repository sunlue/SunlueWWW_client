<template>
  <footer class="clearfix">
    <div class="footer-logo float-left">
      <img class="img-fluid" src="~/assets/images/footerLogo.png" alt="">
    </div>
    <div class="clearfix float-left item-box">
      <div class="float-left list-item">
        <p class="name">地址</p>
        <p class="value"><a href="https://www.gaode.com/" target="_blank">{{address}}</a></p>
      </div>
      <div class="float-left list-item">
        <p class="name">电话</p>
        <p class="value"><a :href="'tel:'+tel">{{tel}}</a></p>
      </div>
      <div class="float-left list-item">
        <p class="name">邮箱</p>
        <p class="value"><a :href="'mailto:'+email">{{email}}</a></p>
      </div>
    </div>
    <div class="copy-right">
      Copyright © 四川上略互动网络技术有限公司 {{ICP}}
    </div>
  </footer>
</template>
<script>
  export default {
    data(){
      return{
        address:"绵阳市金家林总部城27栋10楼（四川省军民融合研究院）",
        tel:"400-0304-517",
        email:"service@sunlue.com",
        ICP:"蜀ICP备09004152"
      }
    }
  }
</script>
<style src="~/assets/css/footer.css" scoped></style>

