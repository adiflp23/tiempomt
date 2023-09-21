<template>
  <div class="app">
    <div class="header container h-100 p-5">
      <h1 class="mb-5">El Tiempo</h1>
      <div class="d-flex justify-content-center h-100" >
        <div class="searchbar w-50 mx-2">
          <input type="text" class="input form-control" v-model="ciudad" placeholder="Busca una ciudad...">
        </div>
        <button class="btn-search btn btn-primary" @click="buscarTiempo"><span class="material-icons">search</span></button>
      </div>
    </div>
    <br>
    <Tiempo :ciudad="ciudad" v-if="mostrarTiempo" />
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
    }
  },
  methods: {
    async buscarTiempo () {
      this.mostrarTiempo = false;
      await this.$nextTick();
      this.mostrarTiempo = true;
    }
  }
};
</script>

<style>
.header {
  background-color: rgb(220, 220, 220);
  border-radius: 20px;
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial,;
  margin-top: 5rem;
}

</style>
