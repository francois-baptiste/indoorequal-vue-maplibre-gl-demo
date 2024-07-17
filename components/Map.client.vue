

<template>
  <mgl-map
      :map-style="style"
      :center="center"
      :zoom="zoom"
      :hash="false"
      height="500px"
      @map:mousemove="onMove"
  >
    <mgl-geo-json-source source-id="states" :data="earthquakesUrl">
      <mgl-fill-layer layer-id="state-fills" :paint="{
      'fill-color': '#627BC1',
      'fill-opacity': [
        'case',
        ['boolean', ['feature-state', 'hover'], false],
        1,
        0.5
      ]
    }" />
      <mgl-line-layer layer-id="state-borders" :paint="{
      'line-color': '#627BC1',
      'line-width': 2
    }" />

    </mgl-geo-json-source>
    <mgl-navigation-control />
  </mgl-map>
</template>

<script setup lang="ts">

const { center } = defineProps({ center: Object, zoom: Number  });

import "maplibre-gl/dist/maplibre-gl.css";
import {
  MglMap,
  MglNavigationControl,
  MglGeoJsonSource,
  MglFillLayer,
  MglLineLayer
} from '@indoorequal/vue-maplibre-gl';

const style = 'https://demotiles.maplibre.org/style.json';
const earthquakesUrl = 'https://maplibre.org/maplibre-gl-js/docs/assets/us_states.geojson';

let hoveredStateId = null;


function onMove(e) {
  const features = e.map.queryRenderedFeatures(e.event.point, {
    layers: ['state-fills'],
  })
  if (features.length > 0) {
    if (hoveredStateId) {
      e.map.setFeatureState(
          {source: 'states', id: hoveredStateId},
          {hover: false}
      );
    }
    hoveredStateId = features[0].id;
    e.map.setFeatureState(
        {source: 'states', id: hoveredStateId},
        {hover: true}
    );
  }
}

</script>

