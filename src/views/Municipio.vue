<script setup>
    import { onMounted, ref, computed } from 'vue';
    import ClienteService from '@/services/ClienteService';
    import RouterLink  from '../components/UI/RouterLink.vue';
    import Heading from '../components/UI/Heading.vue';

    const clientes = ref([])

    onMounted(() => {
        ClienteService.obtenerClientes()
            .then(({data}) => {
                clientes.value = data
            })
            .catch(error => console.log('Hubo un error'))
        })

    defineProps({
        titulo:{
        type: String
        }
    })


</script>

<!--<template>
        <div class="flex justify-end">
            <RouterLink to="municipio">
                Volver
            </RouterLink>
        </div>
        <Heading>{{ titulo }}</Heading>
        <div id="app" class="p-6">
    <h1 class="text-2xl font-extrabold text-slate-500">Acciones</h1>
    <div class="mb-6 flex gap-4">
      <button class="bg-blue-500 text-white px-4 py-2 rounded-lg shadow-md hover:bg-blue-600 focus:outline-none w-48">
        Censistas
      </button>
      <button class="bg-green-500 text-white px-4 py-2 rounded-lg shadow-md hover:bg-green-600 focus:outline-none w-48">
        Relevamientos
      </button>
      <button class="bg-red-500 text-white px-4 py-2 rounded-lg shadow-md hover:bg-red-600 focus:outline-none w-48">
        Intervenciones
      </button>
    </div>
    
   
    <div class="grid grid-cols-3 gap-4">
      <div class="bg-gray-200 p-6 rounded-lg shadow-md">
        <p class="text-center text-8px font-semibold">Cantidad Árboles relevados</p>
      </div>
      <div class="bg-gray-200 p-6 rounded-lg shadow-md">
        <p class="text-center text-8px font-semibold">Absorción Co2</p>
      </div>
      <div class="bg-gray-200 p-6 rounded-lg shadow-md">
        <p class="text-center text-8px font-semibold">Especies de árboles</p>
      </div>
    </div>
    
  </div>
  
    
</template>-->

<template>
  <div class="flex flex-col min-h-screen">
    <!-- Contenedor principal -->
    <div class="flex flex-col flex-1 p-6">
      <!-- Botón "Volver" -->
      <div class="flex justify-end mb-4">
        <RouterLink to="municipio" class="text-blue-500">
          Volver
        </RouterLink>
      </div>
      
      <!-- Título principal -->
      <Heading>{{ titulo }}</Heading>
      
      <!-- Acciones -->
      <h1 class="text-2xl font-extrabold text-slate-500 mb-6">Acciones</h1>
      <div class="mb-6 flex gap-4">
        
          <RouterLink to="inicio">
            Censistas
          </RouterLink>
        
        
          <RouterLink to="relevamientos">
            Relevamientos
          </RouterLink>
        
          <RouterLink to="intervenciones">
            Intervenciones
          </RouterLink>
      </div>
      
      <!-- Recuadros pequeños -->
      <div class="grid grid-cols-3 gap-4">
        <div class="bg-gray-200 p-6 rounded-lg shadow-md">
          <p class="text-center text-xs font-semibold">Cantidad Árboles relevados</p>
        </div>
        <div class="bg-gray-200 p-6 rounded-lg shadow-md">
          <p class="text-center text-xs font-semibold">Absorción Co2</p>
        </div>
        <div class="bg-gray-200 p-6 rounded-lg shadow-md">
          <p class="text-center text-xs font-semibold">Especies de árboles</p>
        </div>
      </div>
    </div>
    
    <!-- Footer -->
   
  </div>
</template>
