<template>
  <div id="cesiumContainer"></div>
</template>

<script>
import * as Cesium from 'cesium';
window.CESIUM_BASE_URL = "./Cesium"
import "cesium/Build/Cesium/Widgets/widgets.css";
import CustomProvider from "./js/CustomProvider";

export default {
  mounted() {
    this.init();
  },
  methods: {
    async init() {



      const viewer = new Cesium.Viewer("cesiumContainer", {
        geocoder: false, // 搜索功能
        homeButton: false, // home
        sceneModePicker: false, // 隐藏二三维切换
        baseLayerPicker: false, //隐藏默认底图选择控件
        navigationHelpButton: false, // 帮助？号
        animation: false,
        infoBox: false,
        timeline: false,
        fullscreenButton: false,
        // 使用ArcGIS 底图  

      });
      viewer.cesiumWidget.creditContainer.style.display = "none"; // 去除logo
      window.viewer = viewer;

      const options = {
        tileHeight: 256,
        tileWidth: 256,
        maximumLevel: 25,
        minimumLevel: 0,
        tilingScheme: new Cesium.WebMercatorTilingScheme(),
        // tilingScheme: new Cesium.GeographicTilingScheme(),
      }
      let provider = await CustomProvider.fromUrl('https://basemaps.arcgis.com/arcgis/rest/services/World_Basemap_v2/VectorTileServer', options);
      let myProvider = new Cesium.ImageryLayer(provider);
      viewer.imageryLayers.add(myProvider);

      // //
      // viewer.camera.flyTo({
      //   destination: Cesium.Cartesian3.fromDegrees(113.23822194, 23.06025307, 1000),
      //   orientation: {
      //     heading: Cesium.Math.toRadians(0.0),
      //     pitch: Cesium.Math.toRadians(-45.0),
      //   },
      // });
    }

  }
}
</script>

<style lang="less" scoped>
#cesiumContainer {
  width: 100%;
  height: 100%;
}
</style>