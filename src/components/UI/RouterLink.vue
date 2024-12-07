<script setup>
    import { RouterLink } from 'vue-router';

    defineProps({
        to: {
            type: String
        }
    })
</script>
<template>
        <RouterLink 
            :to="{name: to}"
            class="block md:inline-block rounded bg-green-900 py-2 px-3 text-white uppercase font-bold shadow text-sm hover:bg-green-700">
                
            <slot></slot>
        </RouterLink>

</template>