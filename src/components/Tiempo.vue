<template>
  <div class="container p-0">
    <div class="d-flex">
      <div class="card main-div w-100">
        <div class="p-3">
          <h2 class="mb-1 day">Hoy</h2>
          <p class="date mb-0">{{fecha}}</p>
          <small>{{hora}}</small>
          <h2 class="place"><span class="material-icons">{{ nombre }}location_on <small>{{ pais }}</small></span></h2>
          <div class="temp">
            <h1 class="weather-temp">{{temperatura}}&deg;</h1>
            <h5>{{descripcion}} <img :src="iconoUrl"></h5>
          </div>
        </div>
      </div>
      <div class="card card-2 w-100">
        <table class="m-4">
          <tbody>
            <tr>
              <th>Nivel Del Mar</th>
              <td>{{nivelMar}}</td>
            </tr>
            <tr>
              <th>Humedad</th>
              <td>{{humedad}}</td>
            </tr>
            <tr>
              <th>Viento</th>
              <td>{{ viento }}</td>
            </tr>
          </tbody>
        </table>

        <DiasTiempo :ciudadnombre="ciudadnombre"/>

        <div id="div_Form" class="d-flex m-3 justify-content-center">
          <form action="">
            <input
              type="button"
              value="Change Location"
              class="btn change-btn btn-primary"
            />
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import DiasTiempo from "./DiasTiempo.vue";
import axios from 'axios';  
export default {
  name: "miTiempo",
  components: {
    DiasTiempo,
  },
  props: {
    ciudad: String,
  },
  data(){
      return{
        ciudadnombre: this.ciudad,
        temperatura: null,
        descripcion: null,
        iconoUrl: null,
        fecha: null,
        hora: null,
        nombre: null,
        nombreMes: ["Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio", "Julio", "Agosto" ,"Septiembre","Octubre","Noviembre","Diciembre" ],
        pais: null,
        nivelMar: null,
        viento: null,
        humedad: null,
        
      }
  },
  async created() {
    const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.ciudad}&units=metric&appid=502f1c7b80f833988ac49eff956a42e3`)
    const datosTiempo = response.data;
    this.temperatura = Math.round(datosTiempo.main.temp);
    this.descripcion = datosTiempo.weather[0].description;
    this.name = datosTiempo.name;
    this.pais = datosTiempo.sys.country;
    this.viento = datosTiempo.wind.speed;
    this.nivelMar = datosTiempo.main.sea_level;
    this.humedad = datosTiempo.main.humidity;
    this.iconoUrl = `https://api.openweathermap.org/img/w/${datosTiempo.weather[0].icon}.png`;
    const d = new Date();
    this.fecha = d.getDate() + ' ' + this.nombreMes[d.getMonth()] + ' ' + d.getFullYear();
    this.hora = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();

    console.log(datosTiempo);
  }
};
</script>

<style>
.weather-temp {
  margin: 0;
  font-weight: 700;
  font-size: 4em;
}
.h2.mb1.day {
  font-size: 3rem;
  font-weight: 400;
}
.main-div {
  border-radius: 20px;
  color: #fff;
  background-image: url('../assets/fondo.jpg');
  background-size: cover;
  background-position: center;
  background-blend-mode: overlay;
  background-color: black;
  background-repeat: no-repeat;
}
.temp {
  position: absolute;
  bottom: 0;
}

.main-div:hover {
  transform: scale(1.1);
  transition: transform 0.5s ease;
  z-index: 1;
}

.card-2 {
  background-color: #212730;
  border-radius: 20px;
}

.card-details {
  margin-left: 19px;
}
.h1_Left {
  position: absolute;
  bottom: 25px;
  left: 16px;
  font-size: 3vw;
  line-height: 1.2;
}

.h3_Left {
  position: absolute;
  left: 16px;
  font-size: 2vw;
  line-height: 0.5;
}

.h3_Left {
  font-size: 1rem;
}

table {
  position: relative;
  left: 15px;
  border-collapse: separate;
  border-spacing: 15px;
  width: 85%;
  text-align: left;
  max-width: 600px;
  margin: 0 auto;
}

th,
td {
  font-size: 18px;
  color: #000000;
}

td {
  text-align: right;
}

table,
tr:hover {
  color: red;
}

.change_btn {
  background-image: linear-gradient(to right, cyan, magenta);
}
</style>
