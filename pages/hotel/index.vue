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
            var map = new AMap.Map('container');
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