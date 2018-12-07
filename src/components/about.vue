<template>
<div>
<HeaderMain :menuType="currentMenuType"></HeaderMain>
<div class="about-container">
  <div class="about-info">
    <div class="left about-info-l">
      <p>{{address}}</p>
      <p>{{tel}}</p>
      <p>{{code}}</p>
      <p>{{fax}}</p>
    </div>
    <div class="right about-info-r">
      <div class="left about-ewm"><img src="../assets/about-ewm.jpg"/>关注通策　　了解更多</div>
      <div class="left about-online"><img src="../assets/about-online.png"/></div>
    </div>
  </div>  
</div>

<div class="map-container">
  <div class="map" id="M_map"></div>
</div>

<FooterMain></FooterMain>
</div>
</template>

<script>
import HeaderMain from './header'
import FooterMain from './footer'
import LogoBuilding from '@/assets/about-building.png'//导入地图上的建筑图标
export default {
  name: 'about',
  components: {      
      HeaderMain,
      FooterMain
    },
  data () {
    return {
      currentMenuType: 'about',
      address:'地址：浙江省杭州市天目山路327号',
      tel:'电话：0571-88923999',
      code:'邮编：310013',
      fax:'传真：0571-87283502'
    }
  },
  methods: {
    map: function () {
        var map = new BMap.Map("M_map");
        var poi = new BMap.Point(120.10628,30.268531);
        var content = "<div style='margin:0;line-height:25px;padding:2px;'>" +this.address
                      +"<br/>"+this.tel+"<br/>"+this.code+"<br/>" +this.fax+
                    "</div>";
        var myIcon = new BMap.Icon(LogoBuilding, new BMap.Size(143,175));    
        var marker = new BMap.Marker(poi,{icon:myIcon}); 
        map.centerAndZoom(poi, 19);
        map.enableScrollWheelZoom();
        map.addOverlay(marker); 
        //创建检索信息窗口对象
        var searchInfoWindow = null;
        searchInfoWindow = new BMapLib.SearchInfoWindow(map, content, {
            title  : "通策医疗",      //标题
            width  : 290,             //宽度
            height : 105,              //高度
            panel  : "panel",         //检索结果面板
            enableAutoPan : true,     //自动平移
            searchTypes   :[
              BMAPLIB_TAB_SEARCH,   //周边检索
              BMAPLIB_TAB_TO_HERE,  //到这里去
              BMAPLIB_TAB_FROM_HERE //从这里出发
            ]
        });
        marker.addEventListener("click", function(e){
            searchInfoWindow.open(marker);
          });
          searchInfoWindow.open(marker);
    }
  },
  mounted: function () {
    this.map();
  }
}
</script>
