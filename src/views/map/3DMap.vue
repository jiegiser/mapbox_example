<!--
 * @Descripttion:
 * @Author: jiegiser
 * @Date: 2020-03-26 08:37:15
 * @LastEditors: jiegiser
 * @LastEditTime: 2020-03-26 08:55:44
 -->
<template>
  <div id="mapCon" />
</template>

<script>
import mapboxgl from 'mapbox-gl'
export default {
  name: 'HelloWorld',
  created() {
    mapboxgl.accessToken = 'pk.eyJ1IjoiamllZ2lzZXJnZyIsImEiOiJjanExcmJjMTYxMGlxM3hueG9lZjQ4eng5In0.F4Ia4OCMj8HZV8scGQvSfQ'
  },
  mounted() {
    const map = new mapboxgl.Map({
      container: 'mapCon',
      style: 'mapbox://styles/mapbox/light-v10',
      center: [-74.0066, 40.7135],
      zoom: 15,
      pitch: 45,
      bearing: -17.6,
      antialias: true
    })
    map.on('load', () => {
      const layers = map.getStyle().layers
      let labelLayerId
      for (var i = 0; i < layers.length; i++) {
        if (layers[i].type === 'symbol' && layers[i].layout['text-field']) {
          labelLayerId = layers[i].id
          break
        }
      }
      map.addLayer({
        'id': '3d-buildings',
        'source': 'composite',
        'source-layer': 'building',
        'filter': ['==', 'extrude', 'true'],
        'type': 'fill-extrusion',
        'minzoom': 15,
        'paint': {
          'fill-extrusion-color': '#aaa',
          'fill-extrusion-height': [
            'interpolate',
            ['linear'],
            ['zoom'],
            15,
            0,
            15.05,
            ['get', 'height']
          ],
          'fill-extrusion-base': [
            'interpolate',
            ['linear'],
            ['zoom'],
            15,
            0,
            15.05,
            ['get', 'min_height']
          ],
          'fill-extrusion-opacity': 0.6
        }
      }, labelLayerId)
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
