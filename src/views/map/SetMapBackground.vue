<!--
 * @Descripttion:
 * @Author: jiegiser
 * @Date: 2020-03-26 14:23:00
 * @LastEditors: jiegiser
 * @LastEditTime: 2020-03-26 15:56:27
 -->
<template>
  <div id="mapCon" />
</template>

<script>
import mapboxgl from '../mapbox'
export default {
  name: 'WMSMap',
  mounted() {
    new mapboxgl.Map({
      container: 'mapCon',
      style: {
        'version': 8,
        // 设置加载并显示来源的图层信息
        'sources': {
          'wms-layer': {
            'type': 'raster',
            'tiles': [
              'http://172.16.10.132:8010/geoserver/smartgashz/wms?service=WMS&version=1.1.0&request=GetMap&layers=smartgashz:gas_mtpipe_line,gas_mspipe_line,gas_lpipe_line&styles=&bbox={bbox-epsg-3857}&width=256&height=256&srs=EPSG:3857&format=image/png&TRANSPARENT=TRUE'
            ],
            'tileSize': 256
          }
        },
        'layers': [{
          // 图层id，要保证唯一性，注意该图层的顺序，是在下面的图层之下的，因此在缩放的时候才能看见该背景图层
          'id': 'backgroundid',
          'type': 'background',
          // 图形信息
          'paint': {
            // 背景颜色
            'background-color': '#009688',
            // 透明度
            'background-opacity': 0.6
          }
        }, {
          'id': 'wms-layer',
          'type': 'raster',
          'source': 'wms-layer',
          'paint': {}
        }]
      },
      center: [114.41703647375107, 23.10750961303711],
      zoom: 15.99,
      // 倾斜角度，与屏幕面的夹角（0-60）度
      pitch: 45
    })
  }
}
</script>
<style scoped>
#mapCon {
  height: calc(100vh - 85px);
  width: 100%;
}
.selectInfo {
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>
