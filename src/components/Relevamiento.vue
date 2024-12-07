<script setup>
    import { computed } from 'vue';
    import { RouterLink } from 'vue-router';

    const props = defineProps({
        relevamiento:{
            type: Object
        }
    })



   
    

</script>

<template>
    <tr>
        <td class="whitespace-nowrap py-4 px-3 text-sm text-center">
    <p class="text-gray-500">{{ relevamiento.tipoArbol }}</p>
</td>
<td class="whitespace-nowrap py-4 px-3 text-sm text-center">
    <p class="text-gray-500">{{ relevamiento.ubicacion }}</p>
</td>
<td class="whitespace-nowrap py-4 px-3 text-sm text-center">
    <p class="text-gray-500">{{ relevamiento.nombre }}</p>
</td>
<td class="whitespace-nowrap py-4 px-3 text-sm text-center">
    <p class="text-gray-500">{{ relevamiento.apellido }}</p>
</td>

    </tr>
</template>