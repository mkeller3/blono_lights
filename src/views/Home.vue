<template>
  <v-container fluid class="mx-0 px-0 pt-0 h-100">
    <v-row class="mx-0 px-0 mt-0 pt-0">
      <v-col cols="12" class="mx-0 px-0 mt-0 pt-0 hero-banner">
        <v-parallax height="100vh" src="/images/base/banner.jpg" gradient="rgba(0, 0, 0, .52), rgba(0, 0, 0, .52)">
          <div align-center style="padding-top: 25vh; text-align: center;">

            <h1 class="text-h3 white-text">
              Bloomington Normal Christmas Lights
            </h1>

            <v-btn class="ma-5" color="red" variant="outlined" href="#about">
              About
            </v-btn>
          </div>
        </v-parallax>
      </v-col>
      <v-col cols="10" lg="6" class="mx-auto" id="about">
        <h1 class="text-center">About the Lights</h1>
        <p>
          Lorem Ipsum Generator

          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore
          magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
          consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
          pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est
          laborum.</p>
      </v-col>
      <v-col cols="12" class="mx-auto px-0">
        <div id='map' style='width: 100%; height: 400px;'></div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts" setup>
import mapboxgl from "mapbox-gl";
import { onMounted } from 'vue';
// import houses from "/src/houses.json";

const center = { lat: 40.492503311878096, lng: -88.97648031231364 }
onMounted(() => {
  mapboxgl.accessToken = 'pk.eyJ1IjoibWtlbGxlcjMiLCJhIjoiY2xwaHA3YzB6MDVleDJxcGRsaTNkc3JwciJ9.y3YBrNaNcXqF8GzK4LlCXQ';
  const map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mkeller3/clphp8idg00ac01p70m4q4982',
    center: [-88.97648031231364, 40.492503311878096],
    zoom: 11
  });
  map.addControl(new mapboxgl.NavigationControl());
  map.addControl(new mapboxgl.FullscreenControl());
  map.on('load', () => {
    map.addSource('earthquakes', {
      type: 'geojson',
      data: '/blono_lights.geojson'
    });

    map.addLayer({
      'id': 'earthquakes-layer',
      'type': 'circle',
      'source': 'earthquakes',
      'paint': {
        'circle-radius': 4,
        'circle-stroke-width': 2,
        'circle-color': 'red',
        'circle-stroke-color': 'white'
      }
    });
    map.addLayer({
      'id': `labels`,
      'type': 'symbol',
      'source': 'earthquakes',
      'paint': {
        'text-halo-color': '#fff',
        'text-halo-width': 0.5
      },
      'layout': {
        'icon-size': 5, // Adjust the icon size as needed
        'text-field': '{description}',
        'text-font': ['Open Sans Semibold', 'Arial Unicode MS Bold'],
        'text-offset': [0, -2],
        'text-anchor': 'top',
        'text-max-width': 30,
        'text-size': {
          stops: [
            [12, 20], // At zoom level 12, text size is 10
            [15, 25], // At zoom level 18, text size is 16
          ],
        },
      },
      'minzoom': 12

    });
  })
})
</script>

<style>
.white-text {
  color: #fff !important;
}
</style>