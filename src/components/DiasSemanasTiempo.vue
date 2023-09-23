<template>
    <div>
      <h2 class="titulo">Pronóstico para la próxima semana:</h2>
      <b-row>
        <b-col sm="6" md="4" lg="3" xl="2"  v-for="dia in pronosticoDiario" :key="dia.valid_date">
          <b-card class="mb-2 card-background-image">
            <div class="py-3">{{ dia.valid_date }}</div>
            <div class="py-3">{{ dia.min_temp }}°C</div>
            <div class="py-3">{{ dia.max_temp }}</div>
            <div class="py-3">{{ dia.weather.description }}</div>
          </b-card>
        </b-col>
      </b-row>
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
        pronosticoDiario: [],
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
                `https://api.weatherbit.io/v2.0/forecast/daily?lat=${latitud}&lon=${longitud}&key=${apiKeyWeatherbit}`
              )
                .then((response) => response.json())
                .then((data) => {
                  this.pronosticoDiario = data.data.slice(1, 8);
                  console.log(
                    "Pronóstico horario para los próximos 7 días:",
                    this.pronosticoDiario
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
  
  <style>
  .titulo{
    text-align: center;
  }
  .py-3{
    text-align: center;
    color: white;
    
  }
  .card-background-image {
    background-image: url("https://img.freepik.com/foto-gratis/cielo-nubes-blancas_23-2148824914.jpg"); /* Reemplaza 'tu_imagen_de_fondo.jpg' con la URL de tu imagen de fondo */
    background-size: cover;
    background-position: center;
    color: white; 
    
  }
  </style>
  