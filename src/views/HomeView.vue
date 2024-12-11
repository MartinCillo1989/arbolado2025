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

const carousel = ref(null);
const carouselItems = ref(null);

//movimiento de logos
const moveCarousel = () => {
  if (carouselItems.value) {
    const firstItem = carouselItems.value.querySelector("img");
    const itemWidth = firstItem.offsetWidth + 16; 
    const maxScroll = carouselItems.value.scrollWidth - carouselItems.value.offsetWidth;

    const currentTranslate = parseFloat(getComputedStyle(carouselItems.value).transform.split(',')[4]) || 0;
    if (Math.abs(currentTranslate) < maxScroll) {
      carouselItems.value.style.transform = `translateX(${currentTranslate - itemWidth}px)`; 
    } else {
      carouselItems.value.style.transform = `translateX(0px)`;
    }
  }
};

onMounted(() => {
  setInterval(moveCarousel, 3000);
});

</script>

<template>

  <!-- frase inicial y boton de Sign Up -->
  <div id="frase" class="flex flex-col md:flex-row justify-between items-center md:px-44 pt-10 pb-6">
    <p class="text-[#042825] text-2xl md:text-4xl font-bold text-center md:text-left">Municipios <br>  
      <span class="text-[#919192] font-bold">Somos {{ locations.length }} municipios activos contra el cambio climático</span>
    </p>
  </div>

  <!-- Mapa de Argentina -->
  <div id="datos-y-mapa" class="flex flex-col md:px-28 md:flex-row md:justify-evenly items-center md:space-x-6 pt-14">
    <div id="map" class="relative w-4/5 md:w-2/3 h-[700px] md:h-[600px] rounded-2xl overflow-hidden flex flex-col md:flex-row">
      <div id="map-info" class="md:flex-1 md:order-1 md:left-2 h-1/3 md:h-full md:w-1/2 rounded-2xl bg-[#365351] p-6 flex justify-start text-left">
        <div class="md:pt-24 md:pl-6">
          <h5 class="text-[#99a7a6] text-2xl md:text-4xl font-bold opacity-70">Municipios</h5>
          <h3 class="text-white text-3xl md:text-5xl font-bold my-4">Somos {{ locations.length }} municipios activos contra el cambio climático</h3>
        </div>
      </div>

      <div ref="mapElement" class="md:flex-1 md:order-2 md:right-4 bottom-4 md:bottom-0 md:w-1/2 h-full rounded-2xl"></div>
    </div>
    
    <div id="datos" class="flex flex-col w-full md:w-1/5 md:h-[600px] space-y-16 mt-10 md:mt-0">
      <div class="w-full h-32 md:h-40 bg-[#afc199] rounded-2xl p-4 flex items-center justify-between">
        <div class="flex flex-col">
          <p class="font-bold text-3xl md:text-5xl text-[#042825]">500 K</p>
          <p class="text-[#042825] font-medium text-base md:text-3xl">Arboles relevados</p>
        </div>
        <img class="w-16 ml-auto" src="../components/icons/tree-decidious-svgrepo-com.svg" alt="Árbol">
      </div>
      <div class="w-full h-32 md:h-40 bg-[#afc199] rounded-2xl p-4 flex items-center justify-between">
        <div class="flex flex-col">
          <p class="font-bold text-3xl md:text-5xl text-[#042825]">290 T</p>
          <p class="text-[#042825] font-medium text-base md:text-3xl">Absorción de CO2 Aprox</p>
        </div>
        <img class="w-16 ml-auto" src="../components/icons/co2-gas-svgrepo-com.svg" alt="CO2">
      </div>
      <div class="w-full h-32 md:h-40 bg-[#afc199] rounded-2xl p-4 flex items-center justify-between">
        <div class="flex flex-col">
          <p class="font-bold text-3xl md:text-5xl text-[#042825]">500</p>
          <p class="text-[#042825] font-medium text-base md:text-3xl">Especies de árboles</p>
        </div>
        <img class="w-16 ml-auto" src="../components/icons/trees-tree-svgrepo-com.svg" alt="Especies de árboles">
      </div>
    </div>
  </div>

  <!-- Logos de socios -->
  <div id="socios" class="pt-14 pb-36 flex flex-col md:flex-row justify-between items-center md:px-36">
    <h2 class="text-2xl md:w-2/5 md:text-3xl font-semibold text-center p-5 mb-4">
      Socios comprometidos con el cambio climático
    </h2>
    <div
      ref="carousel"
      class="flex items-center justify-start space-x-4 overflow-hidden relative"
    >
      <div
        class="flex transition-all duration-300 ease-in-out"
        ref="carouselItems"
      >
        <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (1).jpg" alt="socios" />
        <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (2).jpg" alt="socios" />
        <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (3).jpg" alt="socios" />
        <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (4).jpg" alt="socios" />
        <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (4).jpg" alt="socios" />
        <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (4).jpg" alt="socios" />
        <img class="h-20 md:h-28 object-contain" src="../assets/img/0-crop (4).jpg" alt="socios" />
      </div>
    </div>
  </div>
  
  <!-- Carousel de datos -->
   <div id="datos-actualizados" class="border-t-2 border-b-2 border-black bg-gradient-to-b from-[#b0c298] to-[#b0c298bd] py-16 md:w-full">
    <div class="flex flex-col pb-10 space-y-2">
      <h2 class="font-bold text-center text-2xl md:text-4xl text-[#042825]">Datos Actualizados</h2>
      <h6 class="font-bold text-center text-2xl md:text-4xl text-[#6a735f]">Totales por municipios activos</h6>
    </div>

    <!-- Primer Carrusel -->
    <Carousel 
      :items="locations" 
      bgColor="#26473c" 
      txColor="#92a29d" 
    />

    <!-- Segundo Carrusel -->
    <Carousel 
      :items="locations" 
      bgColor="#aea646"
      txColor="#6b6951" 
    />
  </div>

  <div class="md:h-40">
  <p class="text-center p-20">Lorem ipsum, dolor sit amet consectetur adipisicing elit. Officiis alias sint odit ipsum ullam fugit error, necessitatibus, quas sequi veritatis voluptatum doloremque atque quasi. Omnis reiciendis neque ducimus suscipit quam!</p>
  <img src="" alt="">
</div>

<div id="fondo" class="w-full py-36 flex flex-col justify-evenly bg-[#042825]">
  <div class="mb-20">
    <p class="text-center text-white px-20">
      Lorem ipsum dolor sit amet consectetur, adipisicing elit. Ad cumque aliquam excepturi temporibus, a repellendus quae labore. Hic, doloremque provident rem, quod accusamus accusantium eligendi ratione ullam ex repellat officiis?
    </p>
  </div>
  <div class="flex flex-wrap justify-center items-center space-x-4">
    <img class="w-28 sm:w-1/4 md:w-28 lg:w-28" src="../components/icons/AppArbolado.svg" alt="">
    <img class="w-28 sm:w-1/4 md:w-28 lg:w-28" src="../components/icons/AppArbolado.svg" alt="">
    <img class="w-28 sm:w-1/4 md:w-28 lg:w-28" src="../components/icons/AppArbolado.svg" alt="">
    <img class="w-28 sm:w-1/4 md:w-28 lg:w-28" src="../components/icons/AppArbolado.svg" alt="">
    <img class="w-28 sm:w-1/4 md:w-28 lg:w-28" src="../components/icons/AppArbolado.svg" alt="">
    <img class="w-28 sm:w-1/4 md:w-28 lg:w-28" src="../components/icons/AppArbolado.svg" alt="">
    <img class="w-28 sm:w-1/4 md:w-28 lg:w-28" src="../components/icons/AppArbolado.svg" alt="">
  </div>
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