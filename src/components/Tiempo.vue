<template>
  <div class="container p-0">
    <div class="d-flex">
      <div class="card main-div">
        <div class="p-3">
          <h2 class="clasehoy day text-light">Hoy</h2>
          <small class="text-light">{{hora}}</small>
          <h2 class="place text-light"><span class="material-icons">{{ nombre }}location_on <small>{{ pais }}</small></span></h2>
          <table>
          <tbody>
            <tr>
              <th>Humedad:</th>
              <td>{{humedad}} %</td>
            </tr>
            <tr>
              <th>Viento:</th>
              <td>{{ viento }} km/h</td>
            </tr>
          </tbody>
        </table>
          <div class="temp">
            <h1 class="weather-temp text-light">{{temperatura}}&deg; Grados</h1>
            <h3 class="weather-descrp text-light">{{ descripcion }}<img :src="iconoUrl"></h3>
            <p class="textodescriptivo">¡Gracias por elegirnos!</p>
          </div>
        </div>
      </div>
      <div class="card card-2">
        <DiasSemanasTiempo :ciudadnombre="ciudadnombre"/>
      </div>
    </div>
    <div class="card card-3 w-100">
        <HorasDiasTiempo :ciudadnombre="ciudadnombre"/>
    </div>
      
  </div>
  
</template>

<script>
import axios from 'axios';  
import HorasDiasTiempo from "./HorasDiasTiempo.vue";
import DiasSemanasTiempo from "./DiasSemanasTiempo.vue";
export default {
  name: "miTiempo",
  components: {
    HorasDiasTiempo,
    DiasSemanasTiempo
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
        viento: null,
        humedad: null,
        
      }
  },
  async created() {
    const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.ciudad}&units=metric&appid=502f1c7b80f833988ac49eff956a42e3&lang=es`)
    const datosTiempo = response.data;
    this.temperatura = Math.round(datosTiempo.main.temp);
    this.descripcion = datosTiempo.weather[0].description;
    this.name = datosTiempo.name;
    this.pais = datosTiempo.sys.country;
    this.viento = datosTiempo.wind.speed;
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
.w-100{
  background-color: transparent;
}
.weather-temp {
  margin: 0;
  font-weight: 700;
  font-size: 65px;
}

div.card.main-div{
  background: rgba(24,24,27, 0.6);
}

.h2.mb1.day {
  font-size: 3rem;
  font-weight: 400;
}
.main-div {
  border-radius: 20px;
  color: #fff;
  background-size: cover;
  background-position: center;
  background: rgba(24,24,27, 0.6);
}
div.card.card-2{
  background-color: transparent;
  border: transparent;
}

.main-div:hover {
  transform: scale(1.1);
  transition: transform 0.5s ease;
  z-index: 1;
}

.card-2 {
  background-color: #212730;
  border-radius: 20px;
  padding-left: 5%;
}

.card-3 {
  background-color: #212730;
  margin-top: 20px;
  text-align: center;
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
.weather-descrp{
  font-size: 45px;
}

.textodescriptivo{
  font-size: 50px;
  color: white;
}

th,
td {
  font-size: 18px;
  color: #ffffff;
  text-align: left;
  
}

td {
  text-align: center;
  color: white;
}

</style>
