<script setup>

        const props = defineProps({
            carro:{
                type: Object,
                required: true
            }
        })
    defineEmits(['agregar-carrito'])
</script>

<template>
    <div  class="col-md-6 col-lg-4 my-4 row align-items-center"  >
                <div class="col-4">
                    <img class="img-fluid" 
                    :src="carro.imagen" 
                    :alt="'imagen carro' + carro.nombre">
                </div>
                <div class="col-8">
                    <h3 class="text-black fs-4 fw-bold text-uppercase">{{ carro.nombre }}</h3>
                    <p>{{ carro.descripcion }}</p>
                    <p class="fw-black text-primary fs-3">${{ carro.precio}}</p>
                    <button
                     type="button"
                      class="btn btn-dark w-100 "
                      @click="$emit('agregar-carrito', carro) "
                     >Agregar al Carrito</button>
                </div>
            </div>
</template>


<style lang="scss" scoped>

</style>