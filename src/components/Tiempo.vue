<template>
  <div class="container p-0">
    <div class="d-flex">
      <div class="card main-div">
        <div class="p-3">
          <h2 class="mb-1 day text-light">Hoy</h2>
          <p class="date mb-0 text-light">{{fecha}}</p>
          <small class="text-light">{{hora}}</small>
          <h2 class="place text-light"><span class="material-icons">{{ nombre }}location_on <small>{{ pais }}</small></span></h2>
          <table class="m-4">
          <tbody>
            <tr>
              <th>Humedad</th>
              <td>{{humedad}} %</td>
            </tr>
            <tr>
              <th>Viento</th>
              <td>{{ viento }} km/h</td>
            </tr>
          </tbody>
        </table>
          <div class="temp">
            <h1 class="weather-temp text-light">{{temperatura}}&deg; Grados</h1>
            <h3 class="text-light">{{descripcion}} <img :src="iconoUrl"></h3>
          </div>
        </div>
      </div>
      <div class="card card-2 w-100">
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
        nivelMar: null,
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
  background-size: cover;
  background-position: center;
  background-image: url('https://images.unsplash.com/photo-1694282303612-111be418eca4?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxfDB8MXxyYW5kb218MHx8fHx8fHx8MTY5NTQ5NzAzNA&ixlib=rb-4.0.3&q=80&w=1080');

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

.card-3 {
  background-color: #212730;
  margin-top: 20px;
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


th,
td {
  font-size: 18px;
  color: #ffffff;
}

td {
  text-align: center;
  color: white;
}

table,
tr:hover {
  color: red;
}
</style>
