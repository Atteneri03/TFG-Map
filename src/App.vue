<script setup>
import { onMounted, ref, toRaw } from "vue";
import L, { marker } from "leaflet";

const lat = ref(0);
const lng = ref(0);
const map = ref();
const mapContainer = ref();
var theMarker = {};
var markers = [];
var LeafIcon = L.Icon.extend({
    options: {
       iconSize:     [45, 45],
       iconAnchor:   [25, 25],
       popupAnchor:  [-3, -76]
    }
});

var greenIcon = new LeafIcon({
    iconUrl: 'green-pointer.webp'
})

onMounted(() => {
  getLocation();
  map.value = L.map(mapContainer.value).setView([51.505, -0.09], 13);
  L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
    maxZoom: 19,
    attribution:
      '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
  }).addTo(map.value);

  map.value.on("click", function(e){
            new L.Marker([e.latlng.lat, e.latlng.lng] , {icon: greenIcon}).addTo(map.value);
            markers.push(e.containerPoint);
         });

});

function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.watchPosition((position) => {
      lat.value = position.coords.latitude;
      lng.value = position.coords.longitude;
      map.value.setView([lat.value, lng.value], 13);

      if (theMarker != undefined) {
              map.value.removeLayer(theMarker);
        };

     theMarker = L.marker([lat.value, lng.value],{draggable : false})
     .addTo(toRaw(map.value));
      


    });
  }
}

</script>

<template>
  
<div>
  <div ref="mapContainer" style="width: 400px; height: 400px"></div>
</div>

</template>

<style scoped>
</style>
