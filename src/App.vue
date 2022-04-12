<template>
  <div id="app">
    <l-map :zoom="10" :center="mapCenter" 
      :minZoom="8"
      :maxZoom="18"
      :options="{zoomControl: false, attributionControl: false}">
      <!-- <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer> -->
      <!-- 图层切换 -->
      <l-control-layers position="topright"></l-control-layers>
      <l-tile-layer
          v-for="item in tileProviders"
          :key="item.name"
          :name="item.name"
          :visible="item.visible"
          :url="item.url"
          :attribution="item.attribution"
          class="icon-rotate"
          layer-type="base">
      </l-tile-layer>
      <!-- 控制 zoom 右下角 放大缩小 -->
      <l-control-zoom position="bottomright" :zoomInTitle="zoomInTitle" :zoomOutTitle="zoomOutTitle"></l-control-zoom>



      <!-- 画扇形区域 -->
      <v-semicircle
        v-for="(item, index) in semiCircle"
        :key="'semicircle-' + index"
        :latLng="[item.location.lat, item.location.lng]"
        :options="item.options"></v-semicircle>
    </l-map>
  </div>
</template>

<script>
import Vue from "vue";
import { latLng } from "leaflet";
import { LMap, LTileLayer, LControlLayers, LControlZoom } from "vue2-leaflet";

// import Vue2LeafletSemicircle from 'vue2-leaflet-semicircle' // this is for production
import Vue2LeafletSemicircle from "./Vue2LeafletSemicircle"; // this is used in development to work on the source

Vue.component("v-semicircle", Vue2LeafletSemicircle);

export default {
  name: "app",
  components: {
    LMap,
    LTileLayer,
    LControlLayers,
    LControlZoom
  },
  data() {
    return {
      mapCenter: [37.61991604728149, 84.20471191406251],
      center: latLng(37.61991604728149, 84.20471191406251), //若羌县中心
      tileProviders: [
        {
          name: '行政区划图',
          visible: true,
          url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        },
        {
          name: '卫星图',
          visible: false,
          url: 'https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png',
        },
      ],
      zoomInTitle: '放大',
      zoomOutTitle: '缩小',
      // 扇形区域数据
      semiCircle: [
        // 北右
        {
          location: latLng(37.61991604728149, 84.20471191406251),
          options: {
            startAngle: 45,
            stopAngle: 90,
            radius: 4000,
          },
        }, 
        // 正下
        // {
        //   location: latLng(37.61991604728149, 84.20471191406251),
        //   options: {
        //     startAngle: 135,
        //     stopAngle: 180,
        //     radius: 3000,
        //   },
        // }, 
        // 下左
        {
          location: latLng(37.61991604728149, 84.20471191406251),
          options: {
            startAngle: 225,
            stopAngle: 270,
            radius: 4000,
          },
        }, 
        // 左上
        // {
        //   location: latLng(37.61991604728149, 84.20471191406251),
        //   options: {
        //     startAngle: 315,
        //     stopAngle: 360,
        //     radius: 5000,
        //   },
        // }
      ],
    };
  },
};
</script>

<style>
/* @import "~leaflet/dist/leaflet.css"; */
* {
  padding: 0px;
}
#app {
  width: 100%;
  height: 90vh;
}
</style>
