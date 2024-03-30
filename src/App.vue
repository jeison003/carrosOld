<script setup>
    import { ref, reactive, onMounted, watch } from 'vue'
    import { db } from './data/carros'
    import Carro from './components/Carro.vue'
    import Header from './components/Header.vue'
    import Footer from './components/Footer.vue'
    
    const carros = ref([]);
    const carrito = ref([]);
    const carro = ref({});

    watch(carrito, ()=>{
        guardarLocalStorage();
    },
    {
        deep:true
    });

    onMounted(() => {
        carros.value = db;
        //en el caso del state con reactive
        // state.guitarras = db
        carro.value = db[3];
        const carritoStorage = localStorage.getItem('carrito');
        if(carritoStorage){
            carrito.value = JSON.parse(carritoStorage);
        }
    })  

    const guardarLocalStorage = () =>{
        localStorage.setItem('carrito',JSON.stringify(carrito.value))
    }
    const agregarCarrito =  (carro)=>{

        const existeCarrito = carrito.value.findIndex( producto => producto.id === carro.id)
        if(existeCarrito >=0){

            carrito.value[existeCarrito].cantidad++
        }else{
            carro.cantidad = 1;
            carrito.value.push(carro);
        }
        
    }

    const decrementarCantidad = (id) =>{
        const index = carrito.value.findIndex( producto => producto.id === id)
        if(carrito.value[index].cantidad <= 1) return
        carrito.value[index].cantidad--
        
    }
    const incrementarCantidad = (id) =>{
        const index = carrito.value.findIndex( producto => producto.id === id)
        if(carrito.value[index].cantidad >= 5) return
        carrito.value[index].cantidad++
        
    }

    const eliminarProducto = (id)=>{
        carrito.value = carrito.value.filter(producto => producto.id !== id)
        
    }
    const vaciarCarrito = ()=>{

        carrito.value = [];
       
    }
</script>

<template>
<!-- Componente Header -->
<Header
    :carrito="carrito"
    :carro="carro"
    @decrementar-cantidad = "decrementarCantidad"
    @incrementar-cantidad = "incrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
/>

    <main class="container-xl mt-5">
        <h2 class="text-center">Carros míticos</h2>

        <div class="row mt-5" style="background-color: gray;">
                <!-- Componentente carro -->
                <Carro 
                
                v-for="carroItem in carros"
                :carro="carroItem"
                @agregar-carrito="agregarCarrito"
                />
        </div>
    </main>
    <Footer/>

</template>

<style scoped></style>
