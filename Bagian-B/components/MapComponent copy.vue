<template>
  <div id="map" style="width: 100%; height: 100vh;"></div>
</template>

<script>
import { onMounted } from "vue";
import maplibregl from "maplibre-gl";

export default {
  name: "MapComponent",
  setup() {
    onMounted(() => {
      const map = new maplibregl.Map({
        container: "map", // The ID of the container element
        style:
        'https://api.maptiler.com/maps/streets/style.json?key=get_your_own_OpIi9ZULNHzrESv6T2vL', // Custom style
        center: [113.9213, -0.7893], // Coordinates of Indonesia's approximate center (Longitude, Latitude)
        zoom: 5, // Zoom level (adjust as needed)
      });
    });
  },
};
</script>

<style scoped>
#map {
  width: 100%;
  height: 100vh;
}
</style>