<template>
  <NuxtLink :to="{ path: '/test', query: { lng: 1.137343,  lat: 47.137451,  zoom: 8 } }">Map</NuxtLink>
  <div>
    <!-- this component will only be rendered on client side -->
    <Map />
  </div>
</template>


