<template>
  <div class="app">
    <div class="header container h-100 p-5">
      <h1 class="mb-5">El Tiempo</h1>
      <div class="d-flex justify-content-center h-100">
        <div class="searchbar w-50 mx-2">
          <input type="text" class="input form-control" v-model="ciudad" placeholder="Busca una ciudad...">
        </div>
        <button class="btn-search btn btn-primary" @click="buscarTiempo"><span class="material-icons">search</span></button>
      </div>
    </div>
    <br>
    <Tiempo :ciudad="ciudad" v-if="mostrarTiempo && mostrarBusqueda" />
    <b-sidebar id="sidebar" right shadow :visible="mostrarLimiteAlcanzado">
      <div class="p-3">
        <h2>Límite de búsquedas alcanzado</h2>
        <p>Para buscar más, adquiera una suscripción</p>
        <p>Para adquirir una suscripcion inicie sesión.</p>
        <p> <a href="/">Inicie sesión o registrese dando click aquí!</a></p>
      </div>
    </b-sidebar>
  </div>
</template>

<script>
import Tiempo from './components/Tiempo.vue'; 

export default {
  name: "App",
  components: {
    Tiempo,
  },
  data() {
    return {
      ciudad: " ",
      mostrarTiempo: false,
      contadorBusquedas: 0, // Agregar contador de búsquedas
      mostrarBusqueda: true, // Inicializar mostrarBusqueda
      mostrarLimiteAlcanzado: false, // Inicializar mostrarLimiteAlcanzado
    }
  },
  methods: {
    async buscarTiempo() {
      if (this.contadorBusquedas < 5) {
        this.mostrarTiempo = false;
        await this.$nextTick();
        this.mostrarTiempo = true;
        this.contadorBusquedas++;
      } else {
        this.mostrarBusqueda = false;
        this.mostrarLimiteAlcanzado = true; // Mostrar el b-sidebar
      }
    }
  }
};
</script>

<style>
body{
  background-image: url("https://images.pexels.com/photos/209831/pexels-photo-209831.jpeg?cs=srgb&dl=pexels-pixabay-209831.jpg&fm=jpg");
  background-size: cover;
  background-repeat: no-repeat;
}
.header {
  border-radius: 20px;
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial,;
  margin-top: 5rem;
  color: rgb(255, 255, 255);
}
</style>
