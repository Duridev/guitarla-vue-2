<script setup>
import { ref, reactive, onMounted } from 'vue';
import { db } from './data/guitarras';
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';

const guitarras = ref([]);
const carrito = ref([]);
const guitarraHeader = ref({})

onMounted(() => {
    guitarras.value = db;
    guitarraHeader.value = db[3]
});

const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)

    if(existeCarrito >= 0) {
        carrito.value[existeCarrito].cantidad++
    } else {
        carrito.value.push(guitarra)
        guitarra.cantidad = 1;
    }
};

const eliminarCarrito = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const vaciarCarrito = () => {
    carrito.value = [];
}

const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id);
    if(carrito.value[index].cantidad <= 1){
        return
    }
    carrito.value[index].cantidad--
}

const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id);
    if(carrito.value[index].cantidad >= 5) return
    carrito.value[index].cantidad++
}

</script>

<template>

    <Header 
        :carrito="carrito"
        :guitarra-header="guitarraHeader"
        @decrementar-cantidad="decrementarCantidad"
        @incrementar-cantidad="incrementarCantidad"
        @agregar-carrito="agregarCarrito"
        @eliminar-carrito="eliminarCarrito"
        @vaciar-carrito="vaciarCarrito"
    />


    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">

            <Guitarra v-for="guitarra in guitarras" 
            :guitarra="guitarra" 
            @agregar-carrito="agregarCarrito" />

        </div>
    </main>

    <Footer />

</template>

<style scoped></style>