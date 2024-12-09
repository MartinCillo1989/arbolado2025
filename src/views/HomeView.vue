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
  <div id="frase" class="flex justify-between px-32 pt-10">
    <p class="text-4xl font-bold">Municipios <br>  
      <span class="text-gray-500 font-bold">
        Somos {{ locations.length }} municipios activos contra el cambio climático
      </span>
    </p>
    <button 
    class="text-white rounded-full w-32 h-10 bg-[#022926] hover:bg-[#022926cb]">
    <RouterLink to="login">
        Sign Up
    </RouterLink>
  </button>
  </div>

  <!-- Mapa de Argentina -->
  <div id="datos-y-mapa" class="flex h-screen justify-evenly pt-10 -mb-40">
    <div id="map" class="relative w-4/5 h-4/5 rounded-2xl overflow-hidden">
      <div id="map-info" class="absolute left-2 h-full w-1/2 rounded-2xl bg-[#365351] backdrop-blur-sm p-10">
        <h5 class="pt-10 text-white text-4xl font-bold opacity-30">Municipios</h5>
        <h3 class="pb-5 text-white text-7xl font-bold my-5">
          Somos {{ locations.length }} municipios activos contra el cambio climático
        </h3>
      </div>

      <div ref="mapElement" class="absolute right-4 w-1/2 h-full rounded-2xl"></div>
    </div>

    <!-- Divs de datos a la derecha -->
    <div id="datos" class="h-4/5 flex flex-col justify-between items-center space-y-4">
      <div class=" w-96 h-full rounded-2xl bg-[#afc199] backdrop-blur-sm p-6 overflow-auto">
        <p class="break-words font-bold text-5xl text-white">{{ locations.length }} Municipios</p>
        <p class="break-words font-bold text-xl text-white"> Lorem ipsum dolor sit amet consectetur adipisicing elit. Consequuntur aliquid voluptate dicta.
        </p>
      </div>
      <div class=" w-96 h-full rounded-2xl bg-[#afc199] backdrop-blur-sm p-6 overflow-auto">
        <p class="break-words font-bold text-5xl text-white">500 T</p>
        <p class="break-words font-bold text-xl text-white"> Lorem ipsum dolor sit amet consectetur adipisicing elit. Consequuntur aliquid voluptate dicta.
        </p>
      </div>
      <div class=" w-96 h-full rounded-2xl bg-[#afc199] backdrop-blur-sm p-6 overflow-auto">
        <p class="break-words font-bold text-5xl text-white">500 T</p>
        <p class="break-words font-bold text-xl text-white"> Lorem ipsum dolor sit amet consectetur adipisicing elit. Consequuntur aliquid voluptate dicta.
        </p>
      </div>
    </div>
  </div>

  <!-- Logos de socios -->
  <div class="mb-28">
    <div id="socios" class="w-full flex items-center h-52 px-36 justify-evenly">
      <div id="text" class="w-1/5">
        <p class="text-3xl">Socios comprometidos con el cambio climático</p>
      </div>

      <!-- IMPORTANTE!!! MODIFICAR A CAROUSEL CUANDO TENGA TODOS LOS LOGOS DEFINITIVOS -->
      <div class="flex items-center justify-evenly p-6 w-4/5 overflow-x-auto scroll-snap-x snap-mandatory scrollbar-hide">
        <img class="object-contain h-28 w-auto p-6 snap-start" src="../assets/img/0-crop (1).jpg" alt="alpa servicios ambientales">
        <img class="object-contain h-28 w-auto p-6 snap-start" src="../assets/img/0-crop (2).jpg" alt="alpa servicios ambientales">
        <img class="object-contain h-28 w-auto p-6 snap-start" src="../assets/img/0-crop (3).jpg" alt="alpa servicios ambientales">
        <img class="object-contain h-28 w-auto p-6 snap-start" src="../assets/img/0-crop (4).jpg" alt="alpa servicios ambientales">
        <img class="object-contain h-28 w-auto p-6 snap-start" src="../assets/img/0-crop (5).jpg" alt="alpa servicios ambientales">
        <img class="object-contain h-28 w-auto p-6 snap-start" src="../assets/img/0-crop (6).jpg" alt="alpa servicios ambientales">
        <img class="object-contain h-28 w-auto p-6 snap-start" src="../assets/img/0-crop.jpg" alt="alpa servicios ambientales">
      </div>

    </div>
  </div>

  <!-- Carousel de datos -->
   <div id="datos-actualizados" class="bg-[#b0c298] w-screen py-10">
    <div class="flex flex-col space-y-2">
      <h2 class="font-bold text-center text-3xl">Datos Actualizados</h2>
      <h6 class="font-bold text-center text-3xl text-gray-500">Totales por municipios activos</h6>
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

  <div class="h-40">
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