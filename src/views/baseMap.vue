<template>
  <div class="base-map">
    <div id="map">
      <div id="zoom"></div>
      <div id="scale-line"></div>
    </div>
  </div>
</template>

<script>
import { Map, View } from "ol";
import TileLayer from "ol/layer/Tile";
import OSM from "ol/source/OSM";
import XYZ from "ol/source/XYZ";
import Zoom from "ol/control/Zoom";
import ScaleLine from "ol/control/ScaleLine";

export default {
  name: "basemap",
  data() {
    return {};
  },
  created() {},
  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      let baselayer = new TileLayer({
          id: "baseLayer",
          title: "电子底图",
          layerName: "baseLayer",
          source: new XYZ({
            url:
              "http://t4.tianditu.com/DataServer?T=vec_w&tk=bc8607a5baffec68112b0923e618d1a0&x={x}&y={y}&l={z}",
          }),
        }),
        textlayer = new TileLayer({
          id: "markLayer",
          title: "标注层",
          layerName: "markLayer",
          source: new XYZ({
            url:
              "http://t3.tianditu.com/DataServer?T=cva_w&tk=bc8607a5baffec68112b0923e618d1a0&x={x}&y={y}&l={z}",
          }),
        });

      let scaleline = new ScaleLine({
        className: "scale-line-custom",
        target: "scale-line",
        bar: true,
      });

      const map = new Map({
        target: "map",
        layers: [baselayer, textlayer],
        view: new View({
          projection: "EPSG:4326",
          center: [117.22942, 31.79942],
          zoom: 12,
        }),
        controls: [
          new Zoom({
            target: "zoom",
          }),
          scaleline,
        ],
      });
    },
  },
};
</script>

<style scoped lang="less">
.base-map {
  width: 100%;
  height: 100%;
  #map {
    width: 100%;
    height: 100%;
    position: relative;
    #zoom {
      position: absolute;
      right: 20px;
      bottom: 20px;
      width: 40px;
      height: 60px;
      z-index: 99;
    }
    #scale-line {
      position: absolute;
      left: 20px;
      bottom: 16px;
      width: 40px;
      height: 30px;
      z-index: 99;
    }
  }
}
</style>