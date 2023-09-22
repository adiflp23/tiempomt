<template>
    <div class="days-tab text-center">
      <ul class="p-0">
        <li
          v-for="hora in pronosticoHorario"
          :key="hora.timestamp_local"
          class="li_active"
        >
          <div class="py-3">{{ hora.timestamp_local }}:</div>
          <div class="py-3">{{ hora.temp }}°C</div>
          <div class="py-3">- {{ hora.weather.description }}</div>
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
        nombreUbicacion: null,
        pronosticoHorario: [],
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
  
              // Obtener el pronóstico horario y diario de Weatherbit utilizando las coordenadas
              fetch(
                `https://api.weatherbit.io/v2.0/forecast/hourly?lat=${latitud}&lon=${longitud}&key=${apiKeyWeatherbit}`
              )
                .then((response) => response.json())
                .then((data) => {
                  const pronosticoHorario = data.data;
                  console.log(
                    "Pronóstico horario para las próximas 24 horas:",
                    pronosticoHorario
                  );
                })
                .catch((error) => {
                  console.error("Error al obtener datos del tiempo:", error);
                });
  
              fetch(
                `https://api.weatherbit.io/v2.0/forecast/daily?lat=${latitud}&lon=${longitud}&key=${apiKeyWeatherbit}`
              )
                .then((response) => response.json())
                .then((data) => {
                  const pronosticoDiario = data.data;
                  console.log(
                    "Pronóstico diario para los próximos 7 días:",
                    pronosticoDiario
                  );
                })
                .catch((error) => {
                  console.error("Error al obtener datos del tiempo:", error);
                });
            } else {
              console.error(
                "No se encontraron coordenadas para la ubicación proporcionada."
              );
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
  
  <style></style>