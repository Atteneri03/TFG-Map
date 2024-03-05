<script setup>

import { ref , onMounted } from 'vue';
import L from 'leaflet';

const lat = ref(0);
const lng = ref(0);
const map = ref();
const mapContainer = ref();

onMounted(() => {
  getLocation();
 map.value = L.map(mapContainer.value).setView([51.505, -0.09], 13);

  L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
  }).addTo(map.value);

})

function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition((position) => {
      lat.value = position.coords.latitude;
      lng.value = position.coords.longitude;
      map.value.setView([lat.value, lng.value], 13);

      L.marker([lat.value, lng.value],{draggable : false})
      .addTo(map.value);
      // .on("dragend",(event)=> {
      //   lat.value = event.target.getLatLng().lat;
      //   lng.value = event.target.getLatLng().lng;
      // });

    });
  } 
}

</script>

<template>
<p>Latitude: {{ lat }}</p>
<p>Longitude: {{ lng }}</p>
<div ref="mapContainer" style="width: 400px; height: 400px;"></div>
</template>

<style scoped>

</style>
