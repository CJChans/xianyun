<template>
  <div>
    hotel首页
    <div id="container"></div> 
  </div>
</template>

<script>
export default {
    mounted(){
          window.onLoad  = function(){
             var map = new AMap.Map('container',{
                  zoom:14,//级别
                  center: [113.3245904, 23.1066805],//中心点坐标
                  viewMode:'3D'//使用3D视图
            });

            var marker = new AMap.Marker({
                position:[113.3245904, 23.1066805]//位置
            })
            map.add(marker);//添加到地图
      }
          var url = 'https://webapi.amap.com/maps?v=1.4.15&0bb89d19d60f6b6c21251c9751c72444 &callback=onLoad';
          var jsapi = document.createElement('script');
          jsapi.charset = 'utf-8';
          jsapi.src = url;
          document.head.appendChild(jsapi);
    }
}
</script>

<style scoped>
  #container {
      display: block;
      margin: 0 auto;
      width:800px;
      height: 600px;
      }  
</style>