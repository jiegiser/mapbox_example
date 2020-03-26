<!--
 * @Descripttion:
 * @Author: jiegiser
 * @Date: 2020-03-26 09:00:19
 * @LastEditors: jiegiser
 * @LastEditTime: 2020-03-26 09:13:16
 -->
<template>
  <div id="mapCon" />
</template>

<script>
import mapboxgl from 'mapbox-gl'
import indoor from '@/assets/indoor-3d-map.json'
export default {
  name: 'HelloWorld',
  created() {
    mapboxgl.accessToken = 'pk.eyJ1IjoiamllZ2lzZXJnZyIsImEiOiJjanExcmJjMTYxMGlxM3hueG9lZjQ4eng5In0.F4Ia4OCMj8HZV8scGQvSfQ'
  },
  mounted() {
    const map = new mapboxgl.Map({
      container: 'mapCon',
      style: 'mapbox://styles/mapbox/streets-v11',
      center: [-87.61694, 41.86625],
      zoom: 15.99,
      pitch: 40,
      bearing: 20,
      antialias: true
    })
    map.on('load', () => {
      map.addSource('floorplan', {
        'type': 'geojson',
        'data': indoor
      })
      map.addLayer({
        'id': 'room-extrusion',
        'type': 'fill-extrusion',
        'source': 'floorplan',
        'paint': {
          'fill-extrusion-color': ['get', 'color'],
          'fill-extrusion-height': ['get', 'height'],
          'fill-extrusion-base': ['get', 'base_height'],
          'fill-extrusion-opacity': 0.5
        }
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
