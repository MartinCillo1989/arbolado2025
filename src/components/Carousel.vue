<script setup>
    import { ref } from 'vue';

    // Props para recibir la lista de items y colores del carrusel
    defineProps({
        items: { type: Array, required: true },
        bgColor: { type: String, default: '#26473c' } // color predeterminado
    });

    // Configurando el arrastre
    const carousel = ref(null);
    const isDragging = ref(false);
    const startX = ref(0);
    const scrollLeft = ref(0);

    // Métodos para controlar el arrastre
    const startDrag = (e) => {
        isDragging.value = true;
        startX.value = e.pageX - carousel.value.offsetLeft;
        scrollLeft.value = carousel.value.scrollLeft;
    };

    const onDrag = (e) => {
        if (!isDragging.value) return;
        const x = e.pageX - carousel.value.offsetLeft;
        const walk = (x - startX.value) * 2; // velocidad de arrastre
        carousel.value.scrollLeft = scrollLeft.value - walk;
    };

    const stopDrag = () => {
        isDragging.value = false;
    };
</script>

<template>
    <div 
      ref="carousel" 
      class="flex overflow-x-scroll scroll-snap-x snap-mandatory scrollbar-hide space-x-4 p-4"
      @mousedown="startDrag($event)" 
      @mousemove="onDrag($event)" 
      @mouseup="stopDrag" 
      @mouseleave="stopDrag"
    >
      <div 
        v-for="(item, index) in items" 
        :key="index" 
        class="w-80 h-56 rounded-xl shadow-lg flex flex-col items-center justify-center text-white p-6 snap-start"
        :style="{ backgroundColor: bgColor, userSelect: 'none' }"
      >
        <h5 class="text-center text-4xl font-bold pb-3">{{ items.length }}</h5>
        <h4 class="text-center text-l font-semibold p-3">{{ item.name }}</h4>
      </div>
    </div>
  </template>
  
  
  <style scoped>
  /* Ocultar la barra de desplazamiento */
  .scrollbar-hide::-webkit-scrollbar {
    display: none;
  }
  .scrollbar-hide {
    -ms-overflow-style: none; /* IE y Edge */
    scrollbar-width: none; /* Firefox */
  }
  </style>
  