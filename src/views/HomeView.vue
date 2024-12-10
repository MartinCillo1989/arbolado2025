<script setup>
import { ref, onMounted } from "vue";
import { MarkerClusterer } from "@googlemaps/markerclusterer";
import Carousel from '@/components/Carousel.vue';

  //Centrar el mapa en Argentina
const mapElement = ref(null);
const map = ref(null);
const center = ref({ lat: -38.416097, lng: -63.616672 });
const zoom = ref(4);


  //Municipios provisionales
const locations = ref([
  { name: "Rosario", lat: -32.959648123706145, lng: -60.69087085480481 },
  { name: "Baigorria", lat: -32.85850818963751, lng: -60.703092403554244 },
  { name: "Castelar", lat: -31.66846673336747, lng: -62.090651217414205 },
  { name: "Funes", lat: -32.916912, lng: -60.812737 },
  { name: "Villa Gobernador Galvez", lat: -33.031742, lng: -60.640957 },
  { name: "San Lorenzo", lat: -32.749149, lng: -60.741749 },
  { name: "Capitán Bermúdez", lat: -32.820082, lng: -60.718218 },
  
  { name: "Buenos Aires", lat: -34.603722, lng: -58.381592 },
  { name: "Lanús", lat: -34.702636, lng: -58.394783 },
  { name: "Avellaneda", lat: -34.66334, lng: -58.366653 },
  { name: "Quilmes", lat: -34.72904, lng: -58.26374 },
  { name: "San Justo", lat: -34.678493, lng: -58.561646 },
  { name: "La Plata", lat: -34.92145, lng: -57.95453 },
  
  { name: "Cordoba", lat: -31.420083, lng: -64.188776 },
  { name: "Villa Carlos Paz", lat: -31.415897, lng: -64.50412 },
  { name: "Alta Gracia", lat: -31.666138, lng: -64.428257 },
  { name: "Río Cuarto", lat: -33.123867, lng: -64.349196 },
  { name: "Villa María", lat: -32.407459, lng: -63.240539 },
  
  { name: "Mendoza", lat: -32.889459, lng: -68.845838 },
  { name: "Godoy Cruz", lat: -32.892222, lng: -68.845833 },
  { name: "Las Heras", lat: -32.880593, lng: -68.836288 },
  { name: "Guaymallén", lat: -32.907635, lng: -68.827876 },
  { name: "Luján de Cuyo", lat: -33.054115, lng: -68.871007 },
  { name: "Maipú", lat: -32.978931, lng: -68.763473 }
]);

  //Cargar el mapa
onMounted(() => {
  loadGoogleMaps();
});

function loadGoogleMaps() {
  if (window.google) {
    initMap();
  } else {
    const script = document.createElement("script");
    script.src = `https://maps.googleapis.com/maps/api/js?key=AIzaSyAwWgC0Dp1_RRFobXOf1syZ-dOAypz4tks&loading=async&callback=initMap&libraries=visualization,places`;
    script.async = true;
    document.head.appendChild(script);
    window.initMap = initMap;
  }
}

  //Iniciar mapa
function initMap() {
  map.value = new google.maps.Map(mapElement.value, {
    center: center.value,
    zoom: zoom.value,
    disableDefaultUI: true,
  });

  // Crear los marcadores
  const markers = locations.value.map((location) => {
    return new google.maps.Marker({
      position: { lat: location.lat, lng: location.lng },
      title: location.name,
    });
  });

  // Crear el clúster de marcadores
  new MarkerClusterer({ map: map.value, markers: markers });
}


</script>

<template>

  <!-- frase inicial y boton de Sign Up -->
  <div id="frase" class="flex flex-col md:flex-row justify-between items-center md:px-32 pt-10 pb-6">
    <p class="text-2xl md:text-4xl font-bold text-center md:text-left">Municipios <br>  
      <span class="text-gray-500 font-bold">Somos {{ locations.length }} municipios activos contra el cambio climático</span>
    </p>
    <button 
      class="mt-4 md:mt-0 text-white rounded-full px-6 py-2 bg-[#022926] hover:bg-[#022926cb]">
      <RouterLink to="login">
        Sign Up
      </RouterLink>
    </button>
  </div>

  <!-- Mapa de Argentina -->
  <div id="datos-y-mapa" class="flex flex-col md:px-28 md:flex-row justify-between items-center md:space-x-6 pt-14">
    <div id="map" class="relative w-full md:w-3/5 h-96 md:h-[500px] rounded-2xl overflow-hidden">
      <div id="map-info" class="absolute left-2 h-full md:w-1/2 rounded-2xl bg-[#365351] p-6">
        <h5 class="text-white text-2xl md:text-4xl font-bold opacity-70">Municipios</h5>
        <h3 class="text-white text-3xl md:text-5xl font-bold my-4">Somos {{ locations.length }} municipios activos contra el cambio climático</h3>
      </div>
      <div ref="mapElement" class="absolute md:right-4 w-full md:w-1/2 h-full rounded-2xl"></div>
    </div>
    
    <div id="datos" class="flex flex-col w-full md:w-3/12 md:h-[500px] space-y-6 mt-10 md:mt-0">
      <div class="w-full h-32 md:h-40 bg-[#afc199] rounded-2xl p-4">
        <p class="font-bold text-3xl md:text-4xl text-white">{{ locations.length }} Municipios</p>
        <p class="text-white text-base md:text-lg">Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
      </div>
      <div class="w-full h-32 md:h-40 bg-[#afc199] rounded-2xl p-4">
        <p class="font-bold text-3xl md:text-4xl text-white">500 T</p>
        <p class="text-white text-base md:text-lg">Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
      </div>
      <div class="w-full h-32 md:h-40 bg-[#afc199] rounded-2xl p-4">
        <p class="font-bold text-3xl md:text-4xl text-white">500 T</p>
        <p class="text-white text-base md:text-lg">Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
      </div>
    </div>
  </div>

  <!-- Logos de socios -->
    <div id="socios" class="pt-14 pb-36 flex flex-col md:flex-row justify-between items-center md:px-32">
    <h2 class="text-2xl md:w-2/5 md:text-3xl font-bold text-center p-5   mb-4">Socios comprometidos con el cambio climático</h2>
    <div class="flex items-center justify-center space-x-4 overflow-hidden">
      <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (1).jpg" alt="socios">
      <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (2).jpg" alt="socios">
      <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (3).jpg" alt="socios">
      <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (4).jpg" alt="socios">
    </div>
  </div>

  <!-- Carousel de datos -->
   <div id="datos-actualizados" class="bg-gradient-to-b from-[#b0c298] to-[#b0c298bd] py-16 md:w-full">
    <div class="flex flex-col pb-10 space-y-2">
      <h2 class="font-bold text-center text-2xl md:text-3xl">Datos Actualizados</h2>
      <h6 class="font-bold text-center text-2xl md:text-3xl text-gray-500">Totales por municipios activos</h6>
    </div>

    <!-- Primer Carrusel -->
    <Carousel 
      :items="locations" 
      bgColor="#26473c" 
    />

    <!-- Segundo Carrusel -->
    <Carousel 
      :items="locations" 
      bgColor="#aea546" 
    />
  </div>

  <div class="md:h-40">
    <p class="text-center p-20">Lorem ipsum, dolor sit amet consectetur adipisicing elit. Officiis alias sint odit ipsum ullam fugit error, necessitatibus, quas sequi veritatis voluptatum doloremque atque quasi. Omnis reiciendis neque ducimus suscipit quam!</p>
  </div>
</template>

<style>
/* Ocultar la barra de desplazamiento */
.scrollbar-hide::-webkit-scrollbar {
  display: none;
}
.scrollbar-hide {
  -ms-overflow-style: none; /* IE y Edge */
  scrollbar-width: none; /* Firefox */
}
</style>