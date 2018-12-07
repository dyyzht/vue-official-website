<template>
  <header>
    <div class="head">
      <div class="head-logo">
        <a href="/#/"><img src="../assets/logo.jpg"/></a>
      </div>
      <div class="head-info">{{stock}}</div>
      <div class="clear"></div>      
    </div>

    <div class="nav">
      <div class="nav-container">
        <ul>
          <li><a href="/#/" :class="menuType1Selected ? 'on' : '' ">首页</a></li>
          <li><a href="/#/about" :class="menuType2Selected ? 'on' : '' ">关于我们</a></li>
        </ul>
      </div>
    </div>

    <div class="banner">
         <div class="box">
             <div class="env-wrap" id="J_Banner">
                 <div id="J_BannerItems">
                     <div class="env env1 selected J_BannerItem" data-page="1"></div>
                     <div class="env env2 J_BannerItem"  data-page="2"></div>
                     <div class="env env3 J_BannerItem"  data-page="3"></div>
                     <div class="env env4 J_BannerItem"  data-page="4"></div>
                 </div>
                 <div class="env-nav J_BannerBtn">
                     <p class="selected" data-num="1"></p>
                     <p data-num="2"></p>
                     <p data-num="3"></p>
                     <p data-num="4"></p>
                 </div>
             </div>
         </div>
    </div>

  </header>
</template>

<script>
export default {
  name: 'header',
  props:['menuType'],
  data () {
    return {
      stock: '股票代码 600763  5H'
    }
  },
  methods: {
    banner: function() {
          var t, sp = 2, limit;
          var $bannerItems = $("#J_BannerItems"),
          $bannerItem = $(".J_BannerItem"),
          $bannerBtn = $(".J_BannerBtn");

          limit = $bannerItem.length + 1;

          //实现分页点动态居中的js处理
          if($bannerBtn.length){
            var $pointsElWidth = parseInt($bannerBtn.css('width'))/2;
            $bannerBtn.css('margin-left',-$pointsElWidth);
          }

          //实现图片渐变切换
          var getData = function(page){
            $.each($bannerItem, function(i, n) {
              if((i+1) == page){
                $(this).siblings().fadeOut(400);
                $(this).fadeIn(400);
              }
            });
          }

          //实现页码切换
          var switchCss = function($obj){
            $obj.siblings().removeClass("selected");
            $obj.addClass("selected");
          }

          //分页mouseenter和mouseleave事件处理
          $bannerBtn.delegate("p","mouseenter",function() {
            var $this = $(this);
            switchCss($this);
            getData(parseInt($this.attr("data-num")));

            clearInterval(t);
          });

          $bannerBtn.delegate("p", "mouseleave", function() {
            var $this = $(this);
            sp = parseInt($this.attr("data-num")) + 1;
            if(sp == limit){sp = 1}
            autoSwitch();
          });

          //图片mouseenter和mouseleave事件处理
          $bannerItems.delegate(".J_BannerItem","mouseenter",function() {
            clearInterval(t);
          });

          $bannerItems.delegate(".J_BannerItem", "mouseleave", function() {
            var $this = $(this);
            sp = parseInt($this.attr("data-page")) + 1;
            if(sp == limit){sp = 1}
            autoSwitch();
          });

          var autoSwitch = function(){
            t = setInterval(function(){
              $.each($bannerBtn.find("p"),function(i,n){
                if(parseInt($(n).attr("data-num")) == sp){
                  switchCss($(n));
                }
              });
              getData(sp);
              sp = sp+1;
              if(sp == limit){sp = 1}
            },5000)
          }

          //启动自动切换模式
          autoSwitch();
    }    
  },
  mounted: function () {
    this.banner();
  },
  computed:{
    menuType1Selected(){      
      return this.menuType=='index';
    },
    menuType2Selected(){
      return this.menuType=='about';
    }
  }
}
</script>
