<script setup>
import { ref, onMounted, watch, createVNode, render } from "vue";
import { MarkerClusterer } from "@googlemaps/markerclusterer";

import Alert from "./Alert.vue";

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
      const marker = new google.maps.Marker({
        position: { lat: location.lat, lng: location.lng },
        title: location.name,
        map: map.value,
      });

      // Evento click para mostrar el alert personalizado
      marker.addListener("click", () => {
        showAlert(location);
      });

      return marker;
    });

    // Agrupación de marcadores
    new MarkerClusterer({ map: map.value, markers: markers });
  }
}

// Función para mostrar el alert
function showAlert(location) {
  // Crear un contenedor dinámico para el componente
  const container = document.createElement("div");
  document.body.appendChild(container);

  // Crear el nodo virtual del componente Alert
  const vnode = createVNode(Alert, {
    name: location.name,
    arboles: location.arboles,
    co2: location.co2,
    especies: location.especies,
  });

  // Renderizar el componente en el contenedor
  render(vnode, container);

  // Remover el alert después de 5 segundos
  setTimeout(() => {
    render(null, container); // Limpia el renderizado
    container.remove(); // Elimina el contenedor del DOM
  }, 5000);
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