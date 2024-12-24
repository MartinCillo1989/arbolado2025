<script setup>
    import { ref } from 'vue';

    defineProps({
        items: { type: Array, required: true },
        bgColor: { type: String, default: '#26473c' },
        txColor: { type: String, default: '#91a19c' }
    });

    const carousel = ref(null);
    const isDragging = ref(false);
    const startX = ref(0);
    const scrollLeft = ref(0);

 
    const startDrag = (e) => {
        isDragging.value = true;
        startX.value = e.pageX - carousel.value.offsetLeft;
        scrollLeft.value = carousel.value.scrollLeft;
    };

    const onDrag = (e) => {
        if (!isDragging.value) return;
        const x = e.pageX - carousel.value.offsetLeft;
        const walk = (x - startX.value) * 2;
        carousel.value.scrollLeft = scrollLeft.value - walk;
    };

    const stopDrag = () => {
        isDragging.value = false;
    };
</script>

<template>
    <div 
      ref="carousel" 
      class="flex overflow-x-scroll scroll-snap-x snap-mandatory scrollbar-hide space-x-4 p-4 w-full"
      @mousedown="startDrag($event)" 
      @mousemove="onDrag($event)" 
      @mouseup="stopDrag" 
      @mouseleave="stopDrag"
    >
      <div 
        v-for="(item, index) in items" 
        :key="index" 
        :style="{ backgroundColor: bgColor, userSelect: 'none' }"
        class="md:w-auto md:max-w-[320px] h-36 rounded-3xl shadow-inner-top flex flex-col items-center justify-center px-24 snap-start"
      >
        <h5 class="text-center text-white text-4xl font-bold">360</h5>
        <h4 class="text-center text-3xl font-semibold" :style="{ color: txColor}">municipio</h4>
        <h3 class="text-center text-3xl font-semibold" :style="{ color: txColor}">provincia</h3>
      </div>
    </div>
  </template>
  
  