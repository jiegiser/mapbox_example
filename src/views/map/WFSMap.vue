<!--
 * @Descripttion:
 * @Author: jiegiser
 * @Date: 2020-03-26 15:58:03
 * @LastEditors: jiegiser
 * @LastEditTime: 2020-03-26 15:58:15
 -->
<template>
  <div id="mapCon" />
</template>

<script>
import mapboxgl from '../mapbox'
export default {
  name: 'WMSMap',
  mounted() {
    const map = new mapboxgl.Map({
      container: 'mapCon',
      style: 'mapbox://styles/mapbox/streets-v10',
      center: [114.41703647375107, 23.10750961303711],
      zoom: 15.99
    })
    map.on('load', () => {
      // 第二个参数为添加图层的位置
      map.addLayer({
        'id': 'wms-layer',
        'type': 'raster',
        'source': {
          'type': 'raster',
          'tiles': [
            'http://172.16.10.132:8010/geoserver/smartgashz/wms?service=WMS&version=1.1.0&request=GetMap&layers=smartgashz:gas_mtpipe_line,gas_mspipe_line,gas_lpipe_line&styles=&bbox={bbox-epsg-3857}&width=256&height=256&srs=EPSG:3857&format=image/png&TRANSPARENT=TRUE'
          ],
          'tileSize': 256
        },
        'paint': {}
      })
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
