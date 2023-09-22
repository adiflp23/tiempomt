<template>
  <div>
    <h2 class="titulo">Pronóstico para las próximas 24 horas:</h2>
    <ul>
      <li v-for="hora in pronosticoHorario" :key="hora.timestamp_local">
        <div class="py-3">{{ hora.timestamp_local }}</div>
        <div class="py-3">{{ hora.temp }}°C</div>
        <div class="py-3">{{ hora.weather.description }}</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    ciudadnombre: String,
  },
  data() {
    return {
      nombreUbicacion: this.ciudadnombre,
      pronosticoHorario: [],
    };
  },

  methods: {
    fetchTiempoDatos() {
      // Reemplaza "TU_CLAVE_API_DE_WEATHERBIT" con tu clave API de Weatherbit
      const apiKeyWeatherbit = "41c4999686204f9cb6a2c0689433a1d5";

      // Reemplaza "NOMBRE_DE_LA_UBICACION" con el nombre de la ubicación proporcionado por el usuario
      const nombreUbicacion = this.ciudadnombre;

      // Obtener coordenadas geográficas a partir del nombre de la ubicación con Nominatim
      fetch(
        `https://nominatim.openstreetmap.org/search?format=json&q=${nombreUbicacion}&limit=1&polygon_svg=1&addressdetails=1`
      )
        .then((response) => response.json())
        .then((data) => {
          if (data.length > 0) {
            const latitud = data[0].lat;
            const longitud = data[0].lon;

            // Obtener el pronóstico horario de Weatherbit utilizando las coordenadas
            fetch(
              `https://api.weatherbit.io/v2.0/forecast/hourly?lat=${latitud}&lon=${longitud}&key=${apiKeyWeatherbit}`
            )
              .then((response) => response.json())
              .then((data) => {
                this.pronosticoHorario = data.data;
                console.log(
                  "Pronóstico horario para las próximas 24 horas:",
                  this.pronosticoHorario
                );
              })
              .catch((error) => {
                console.error("Error al obtener datos del tiempo:", error);
              });
          }
        })
        .catch((error) => {
          console.error("Error al obtener coordenadas:", error);
        });
    },
  },
  mounted() {
    this.fetchTiempoDatos();
  },
};
</script>

<style>.days-tab {
  width: 90%;
  box-shadow: 0 4px 8px 0 black, 0 6px 20px 0 black;
  border-radius: 20px;
  width: 90%;
  margin: auto;
}
.loading{
  color: rgb(0, 0, 0);

}
ul{
  margin: 0;

}

li{
  display: inline-block;
  list-style: none;
  height: 100%;
  width: 21%;
  font-size: 1vw;
  line-height: 1.2;
}

span {
  display: block;
  margin-bottom: 5px;
  font: 100% sans-serif;
  height: 35px;
}
.li_active {
  background: #253d5c;
  color: #222831;
  border-radius: 10px;
  margin: 0.5rem;
  color: white;
  font-weight: 600;
}
.li_active:hover{
  transform: scale(1.2);
  transition: transform 0.1s ease;
}
.li_active_temp {
  display: inline-block;
  background-color: #222831;
  color: white;
  transition: background-color 0.5s;
  border-radius: 10px;
}
.li_active_temp:hover {
  transform: scale(1.2);
  transition: transform 0.1s ease;
  background: white;
  border-radius: 10px;
  color: #222831;
}
.titulo{
  text-align: center;
}


</style>
