<template>
  <div id="map" style="width: 100%; height: 100vh;"></div>
</template>

<script setup>
import { onMounted } from 'vue';
import maplibregl from 'maplibre-gl';
import geoJsonDatas from '../server/test.json'
import logoCirclegeo from '../public/images/logo-circlegeo.png'

// Data GeoJSON
const geojsonData = geoJsonDatas

const customMarkerIcon =  '../public/images/logo-circlegeo.png' // Ganti dengan path logo circlegeo.

onMounted(() => {
  // Inisialisasi Map
  const map = new maplibregl.Map({
    container: 'map', // ID elemen HTML
    style: 'https://basemaps.cartocdn.com/gl/voyager-gl-style/style.json', // Style MapLibre
    center: [113.9213, -0.7893], // Coordinates of Indonesia's approximate center (Longitude, Latitude)
    zoom: 5,
  });

  // Layer GeoJSON
  map.on('load', () => {
    //  Line Layer
    map.addSource('line-source', { type: 'geojson', data: geojsonData });
    map.addLayer({
      id: 'line-layer',
      type: 'line',
      source: 'line-source',
      paint: { 'line-color': '#FF0000', 'line-width': 2 },
    });

    //  Polygon Layer (Fill & Outline)
    map.addSource('polygon-source', { type: 'geojson', data: geojsonData });
    map.addLayer({
      id: 'polygon-fill-layer',
      type: 'fill',
      source: 'polygon-source',
      paint: { 'fill-color': '#9d00ff', 'fill-opacity': 0.5 },
    });

    map.addLayer({
      id: 'polygon-outline-layer',
      type: 'line',
      source: 'polygon-source',
      paint: { 'line-color': '#FF0000', 'line-width': 1 },
    });

    //  Circle Layer
    map.addLayer({
      id: 'circle-layer',
      type: 'circle',
      source: 'polygon-source',
      paint: {
        'circle-radius': 10,
        'circle-color': '#0000FF',
      },
      filter: ['==', ['get', 'type'], 'circle'],
    });

    //  Custom Marker
    new maplibregl.Marker({
      element: createCustomMarker(),
    })
      .setLngLat([100.5, 0.5])
      .addTo(map);

    //  Default Marker
    new maplibregl.Marker().setLngLat([101.0, 0.0]).addTo(map);

    //  Vector Tile Layer
    map.addSource('vector-tiles', {
      type: 'raster',
      tiles: [
        'https://rami.bmkg.go.id/api/windtemp_get/wafc/WT/snow/850/202405121800/202405131200/{z}/{x}/{y}.png',
      ],
      tileSize: 256,
    });

    // ADD LAYER
    map.addLayer({
      id: 'vector-tile-layer',
      type: 'raster',
      source: 'vector-tiles',
    });
  });
});

// Fungsi Membuat Custom Marker
function createCustomMarker() {
  const marker = document.createElement('div');
  marker.style.width = '30px';
  marker.style.height = '30px';
  // marker.style.backgroundImage = ⁠ url(${customMarkerIcon}) ⁠;
  marker.style.backgroundSize = 'contain';
  return marker;
}
</script>

<style scoped>
#map {
  position: relative;
}
</style>