<script setup>
  import { RouterView, useRoute } from "vue-router";

  const route = useRoute();
</script>

<template>
  <header class="bg-[rgb(75,101,98)] flex justify-between items-center px-5 py-5">
    <!-- Imagen a la izquierda -->
    <div class="flex-shrink-0 mr-8">
      <img src="./components/icons/Ramcc.svg" alt="Imagen Izquierda" class="h-12">
    </div>
    
    <!-- Imagen centrada -->
    <div>
      <img src="./components/icons/AppArbolado2.png" alt="Imagen Centro" class="h-12 mx-auto">
    </div>

    <!-- Imagen a la derecha -->
    <div class="flex-shrink-0">
      <img src="./components/icons/Fes.svg" alt="Imagen Izquierda" class="h-12">
    </div>
  </header>

  <main :class="route.name === 'home' || route.name === 'municipio' ? '' : 'max-w-6xl mx-auto mt-10'">
    <RouterView />
  </main>
  <footer class="bg-black flex justify-between items-center px-8 h-24 w-full">
      <!-- Contenedor de las dos imágenes de la izquierda con espacio entre ellas -->
      <p class="text-white text-lg font-semibold tracking-wide hover:text-gray-300 transition duration-300">&copy; 2024 App Arbolado</p>
    </footer>
</template>

