<script setup>
    import { ref } from "vue";

    const showAlert = ref(true);

    function closeAlert() {
        showAlert.value = false;
    }
    defineProps({
        name: String,
        arboles: Number,
        co2: String,
        especies: Number,
    });

</script>
<template>
    <div v-if="showAlert" class="fixed bottom-4 left-1/4 bg-[#042825] text-white px-4 pb-3 rounded-lg shadow-lg space-y-4">
     
        <button @click="closeAlert" class="absolute top-3 right-4 text-white text-3xl font-bold">&times;</button>

        <div class="flex justify-center">
            <h1 class="font-bold text-2xl pb-10">Municipio de {{ name }}</h1>
        </div>

        <div class="flex space-x-4">
            <div class="w-full bg-[#afc199] rounded-2xl p-4 flex items-center shadow-inner-top">
                <div class="flex flex-col">
                    <p class="font-bold text-2xl text-[#042825]">{{ arboles }}</p>
                    <p class="text-[#042825] font-medium text-sm">Árboles relevados</p>
                </div>
                <img class="w-12 ml-auto" src="../components/icons/tree-decidious-svgrepo-com.svg" alt="Árbol" />
            </div>
            <div class="w-full bg-[#afc199] rounded-2xl p-4 flex items-center shadow-inner-top">
                <div class="flex flex-col">
                    <p class="font-bold text-2xl text-[#042825]">{{ co2 }}</p>
                    <p class="text-[#042825] font-medium text-sm">Absorción de CO2 Aprox</p>
                </div>
                <img class="w-12 ml-auto" src="../components/icons/co2-gas-svgrepo-com.svg" alt="CO2" />
            </div>
            <div class="w-full bg-[#afc199] rounded-2xl p-4 flex items-center shadow-inner-top">
                <div class="flex flex-col">
                    <p class="font-bold text-2xl text-[#042825]">{{ especies }}</p>
                    <p class="text-[#042825] font-medium text-sm">Especies de árboles</p>
                </div>
                <img class="w-12 ml-auto" src="../components/icons/trees-tree-svgrepo-com.svg" alt="Especies de árboles" />
            </div>
        </div>
    </div>
</template>