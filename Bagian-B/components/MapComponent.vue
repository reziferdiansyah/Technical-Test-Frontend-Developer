<template>
  <div ref="mapContainer" class="map-container"></div>
</template>

<script>
import { ref, onMounted } from 'vue';
import maplibre from 'maplibre-gl';

export default {
  name: 'MapComponent',
  setup() {
    const mapContainer = ref(null);  // Reference to the map container element
    // Initialize map after component is mounted
    onMounted(() => {
      new maplibre.Map({
        container: mapContainer.value, // Attach the map to the container
        style: 'https://demotiles.maplibre.org/style.json', // Example style (you can use a custom style)
        center: [0, 0], // Initial map center [longitude, latitude]
        zoom: 2, // Initial zoom level
      });
    });
    return {
      mapContainer,
    };
  },
};
</script>

<style scoped>
.map-container {
  width: 100%;
  height: 100vh; /* Full screen map */
}
</style>
