<template>
  <NuxtLink :to="{ path: '/', query: { lng: -110.,  lat: 40.137451,  zoom: 8 } }">Map</NuxtLink>
  <div>
    <!-- this component will only be rendered on client side -->
    <Map :center="mapCenter" :zoom="zoom" />
  </div>
</template>


<script setup>
import { ref, computed } from 'vue';

import 'maplibre-gl/dist/maplibre-gl.css';

const route = useRoute()
const lat = parseFloat(route.query.lat) ? parseFloat(route.query.lat) : 37.
const lng = parseFloat(route.query.lng) ? parseFloat(route.query.lng) : -100.
const zoom = parseFloat(route.query.zoom) ? parseFloat(route.query.zoom) : 3

const mapCenter = computed(() => [lng, lat]);



</script>
