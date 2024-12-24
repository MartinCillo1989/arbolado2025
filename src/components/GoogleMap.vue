<script setup>
  import { ref, onMounted, watch } from "vue";
  import { MarkerClusterer } from "@googlemaps/markerclusterer";
  
  const props = defineProps({
    center: {
      type: Object,
      required: true,
    },
    zoom: {
      type: Number,
      required: true,
    },
    locations: {
      type: Array,
      required: true,
    },
  });
  
  const mapElement = ref(null);
  const map = ref(null);
  
  onMounted(() => {
    loadGoogleMaps();
  });
  
  function loadGoogleMaps() {
    if (window.google) {
      initMap();
    } else {
      const script = document.createElement("script");
      script.src = `https://maps.googleapis.com/maps/api/js?key=AIzaSyAwWgC0Dp1_RRFobXOf1syZ-dOAypz4tks&callback=initMap&libraries=visualization,places`;
      script.async = true;
      document.head.appendChild(script);
      window.initMap = initMap;
    }
  }
  
  function initMap() {
    map.value = new google.maps.Map(mapElement.value, {
      center: props.center,
      zoom: props.zoom,
      disableDefaultUI: true,
    });
  
    updateMarkers();
  }
  
  function updateMarkers() {
    if (map.value) {
      const markers = props.locations.map((location) => {
        return new google.maps.Marker({
          position: { lat: location.lat, lng: location.lng },
          title: location.name,
        });
      });
  
      new MarkerClusterer({ map: map.value, markers: markers });
    }
  }
  
  watch(
    () => [props.center, props.zoom, props.locations],
    () => {
      if (map.value) {
        map.value.setCenter(props.center);
        map.value.setZoom(props.zoom);
        updateMarkers();
      }
    }
  );
</script>
  
<template>
    <div ref="mapElement" class="w-full h-full"></div>
</template> 