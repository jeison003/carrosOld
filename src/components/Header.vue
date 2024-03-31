<script setup>
import {computed} from 'vue'
import {formatearCantidad} from '../helpers'
    const props = defineProps({
        carrito: {
            type: Array,
            required: true
        },
        carro:{
            type: Object,
            required: true
        }
    })

    defineEmits(['incrementar-cantidad', 'decrementar-cantidad', 'agregar-carrito','eliminar-producto', 'vaciar-carrito'])

    const totalPagar = computed(()=>{
        return props.carrito.reduce((total, producto)=> total + (producto.cantidad * producto.precio), 0)
    })
</script>

<template>
  
  <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid logoCar" src="https://github.com/jeison003/carrosOld/blob/main/public/img/logoCar.png?raw=true" alt="imagen logo">
                    </a>
                </div>
                
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div class="carrito">
                        <img class="img-fluid" src="https://github.com/jeison003/carrosOld/blob/main/public/img/carrito.png?raw=true" alt="imagen carrito" />

                        <div id="carrito" class="bg-white p-1 ">
                            <p v-if="carrito.length === 0" class="text-center m-0">
                                El carrito esta vacio
                            </p>

                            <div class="navCar" v-else> 
                                <table  class="w-100 table">
                                    <thead>
                                        <tr class="trResponsive">
                                            <th>Imagen</th>
                                            <th>Nombre</th>
                                            <th>Precio</th>
                                            <th>Cantidad</th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr
                                        v-for="producto in carrito"
                                        class="trResponsive"
                                        >
                                            <td >
                                                <img class="carImgCarrito" :src="producto.imagen" alt="imagen carro">
                                            </td>
                                            <td >
                                                <p>{{ producto.nombre }}</p>
                                            </td>
                                            <td class="fw-bold ">
                                                <p>{{ formatearCantidad(producto.precio) }}</p>
                                            </td>
                                            <td class="flex gap-4">
                                                <button 
                                                
                                                @click="$emit('decrementar-cantidad', producto.id)"
                                                type="button"
                                                class="btn btn-dark buttonResponsive">
                                                    -
                                                </button>

                                                {{ producto.cantidad }}

                                                <button  
                                                @click="$emit('incrementar-cantidad', producto.id)"
                                                type="button" 
                                                class="btn btn-dark buttonResponsive">
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button 
                                                class="btn btn-danger deleteButton" 
                                                type="button"
                                                @click="$emit('eliminar-producto',producto.id)"
                                                >
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>

                                <p class="text-end">Total pagar: <span class="fw-bold">{{ formatearCantidad(totalPagar) }}</span></p>
                                <button 
                                class="btn btn-dark w-100 mt-3 p-2"
                                @click="$emit('vaciar-carrito')"
                                >
                                Vaciar Carrito</button>
                        </div>
                    </div>
                </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">{{ carro.nombre }}</h1>
                    <p class="mt-5 fs-5 text-white">{{ carro.descripcion }}.</p>
                    <p class="text-primary fs-1 fw-black">${{ formatearCantidad(carro.precio) }}</p>
                    <button 
                    type="button" 
                    class="btn fs-4 bg-primary text-white py-2 px-5"
                    @click="$emit('agregar-carrito', carro)"
                    >
                    Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-carro" src="https://github.com/jeison003/carrosOld/blob/main/public/img/bmw_e36bgheader.png?raw=true" alt="imagen header">
    </header>
</template>



<style lang="scss" scoped>

</style>