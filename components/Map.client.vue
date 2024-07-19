

<template>
  <mgl-map
      :map-style="styleUrl"
      :center="center"
      :zoom="zoom"
      :hash="false"
      height="500px"
      @map:mousemove="onMove"
  >
<!--    <mgl-vector-source source-id="states" :url="styleUrl">-->

<!--    </mgl-vector-source>-->

<!--    <mgl-navigation-control />-->
    <InspectControl />
  </mgl-map>
</template>

<script setup lang="ts">

const { center } = defineProps({ center: Object, zoom: Number  });
import '@maplibre/maplibre-gl-inspect/dist/maplibre-gl-inspect.css';
import "maplibre-gl/dist/maplibre-gl.css";
import {
  MglMap,
  MglNavigationControl,
  MglGeoJsonSource,
  MglVectorSource,
  MglFillLayer,
  MglLineLayer,
  useControl,
} from '@indoorequal/vue-maplibre-gl';

import MaplibreInspect from '@maplibre/maplibre-gl-inspect';
import maplibregl from 'maplibre-gl';

const styleUrl = 'https://www.zeroimmo.com/toto.json';


const earthquakesUrl = 'https://maplibre.org/maplibre-gl-js/docs/assets/us_states.geojson';

let hoveredStateId = null;


function onMove(e) {
  const features = e.map.queryRenderedFeatures(e.event.point, {
    layers: ['countries-fill'],
  })
  if (features.length > 0) {
    if (hoveredStateId) {
      e.map.setFeatureState(
          {source: 'maplibre', sourceLayer: 'countries', id: hoveredStateId},
          {hover: false}
      );
    }
    hoveredStateId = features[0].id;
    e.map.setFeatureState(
        {source: 'maplibre', sourceLayer: 'countries', id: hoveredStateId},
        {hover: true}
    );
  }
}

const InspectControl = defineComponent({
  setup() {
    return useControl(
        () => {
          return new MaplibreInspect({
            popup: new maplibregl.Popup({
              closeButton: false,
              closeOnClick: false
            })
          });
        }, {
          position: 'top-right'
        });
  },

  render() {
    return null;
  }
});

</script>

