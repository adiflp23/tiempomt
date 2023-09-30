<template>
    <div class="diasSemanas">
      <h2 class="titulo">Pronóstico para la próxima semana</h2>
      <b-row>
        <b-col sm="6" md="4" lg="2" xl="3"  v-for="dia in pronosticoDiario" :key="dia.valid_date">
          <b-card class="mb-2 card-background-image">
            <div class="py-1">{{ mostrarDia(dia.valid_date) }}</div>
            <div class="py-2">Min {{ dia.min_temp }}°C</div>
            <div class="py-2">Máx {{ dia.max_temp }}°C</div>
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
                `https://api.weatherbit.io/v2.0/forecast/daily?lat=${latitud}&lon=${longitud}&key=${apiKeyWeatherbit}&lang=es`
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
      mostrarDia(timestamp) {
      // Formatea el día para mostrar solo el nombre del día (en español) sin coma
      const date = new Date(timestamp);
      const dayName = date.toLocaleDateString('es-ES', { weekday: 'long' });
      return dayName.charAt(0).toUpperCase() + dayName.slice(1);
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
    color: white;
    margin-bottom: 2rem;
    font-size: 40px;
  }
.py-1{
  text-align: center;
  color: white;
  font-size: 23px;
}

.py-2{
    text-align: center;
    color: white;
    
}
.py-3{
    text-align: center;
    color: white;
    
}
  .card-background-image {
    background-image: url("https://cdn5.f-cdn.com/contestentries/329593/18088126/5699feb841990_thumb900.jpg"); /* Reemplaza 'tu_imagen_de_fondo.jpg' con la URL de tu imagen de fondo */
    background-size: cover;
    background-position: center;
    color: white; 
    
  }
  </style>
  