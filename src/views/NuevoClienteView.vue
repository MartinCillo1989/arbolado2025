<script setup>
    import { FormKit } from '@formkit/vue';
    import { useRouter } from 'vue-router';
    import ClienteService from '@/services/ClienteService';
    import RouterLink  from '../components/UI/RouterLink.vue';
    import Heading from '../components/UI/Heading.vue';

    
    const router = useRouter()

    

    defineProps({
        titulo:{
        type: String
        }
    })

    const handleSubmit= (data) => {
        data.estado = 1
        ClienteService.agregarCliente(data)
            .then(respuesta => {
                console.log(respuesta)
                router.push('/')
            })
            .catch(error => console.log(error))
    }

</script>

<template>
    <div>
        <div class="flex justify-end">
            <RouterLink to="inicio"
            >
                Volver
            </RouterLink>
        </div>
        <Heading>{{titulo}}</Heading>

        <div class="mx-auto mt-10 bg-white shadow">
            <div class="mx-auto md:w-2/3 py-20 px-6">
            <FormKit
                type="form"
                submit-label="Agregar censista"
                incomplete-message="No se pudo enviar, revisa los mensajes"
                @submit="handleSubmit"
            >
                <FormKit 
                    type="text"
                    label="Nombre"
                    name="nombre"
                    placeholder="Nombre de cliente"
                    help="Coloca el nombre del cliente que deseas registrar"
                    validation="required"
                    :validation-messages="{ required: 'El nombre del censista es obligatorio'}"
                />
                <FormKit 
                    type="text"
                    label="Apellido"
                    name="apellido"
                    placeholder="Apellido de cliente"
                    validation="required"
                    :validation-messages="{ required: 'El apellido del censista es obligatorio'}"
                />
                <FormKit 
                    type="email"
                    label="Email"
                    name="email"
                    placeholder="Email de cliente"
                    validation="required|email"
                    :validation-messages="{ required: 'El email del censista es obligatorio', email:'Coloca un email valido'}"
                />
                <FormKit 
                    type="text"
                    label="Teléfono"
                    name="telefono"
                    placeholder="Teléfono: xxx-xxx-xxxx"
                    validation="?matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
                    :validation-messages="{matches:'El formato no es valido'}"
                />
                <FormKit 
                    type="text"
                    label="Empresa"
                    name="empresa"
                    placeholder="Empresa de cliente"
                />
                <FormKit 
                    type="text"
                    label="Puesto"
                    name="puesto"
                    placeholder="Puesto de cliente"
                />
                

            </FormKit>
            </div>
        </div>
    </div>
</template>

<style>
    .formkit-wrapper {
        max-width: 100%;

    }
</style>