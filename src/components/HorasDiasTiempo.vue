<template>
  <div class="table-container">
    <div class="table-scroll">
      <b-table
        sticky-header
        responsive
        :items="pronosticoHorario"
        :fields="fields"
        class="table-scroll-horizontal"
      >
        <template #cell(timestamp_local)="row">
          {{ formatHour(row.item.timestamp_local) }}
        </template>
        <template #cell(temp)="row"> 
          {{ row.item.temp }}°C 
        </template>
        <template #cell(description)="row">
          {{ row.item.weather.description }}
        </template>
      </b-table>
    </div>
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
      fields: [
        { key: "timestamp_local", label: "Hora" },
        { key: "temp", label: "Temperatura (°C)" },
        { key: "weather.description", label: "Descripción del Tiempo" },
      ],
      stickyHeader: true,
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
              `https://api.weatherbit.io/v2.0/forecast/hourly?lat=${latitud}&lon=${longitud}&key=${apiKeyWeatherbit}&lang=es`
            )
              .then((response) => response.json())
              .then((data) => {
                this.pronosticoHorario = data.data.slice(0, 24);
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
    formatHour(timestamp) {
      // Formatea la hora para mostrar solo la hora y minutos (HH:mm)
      const date = new Date(timestamp);
      return date.toLocaleTimeString("es-ES", {
        hour: "2-digit",
        minute: "2-digit",
      });
    },
    
  },
  mounted() {
    this.fetchTiempoDatos();
  },
};
</script>
<style>
.table-container {
  margin: 20px;
}

.table-scroll {
  overflow-x: auto;
}

.table-scroll-horizontal {
  width: 100%;
  border-collapse: collapse;
}

.b-table {
  background-color: #ffffff;
  border: 1px solid #e5e5e5;
  border-radius: 4px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.b-table th,
.b-table td {
  padding: 12px;
  text-align: center;
}

.b-table th {
  background-color: #f5f5f5;
  color: #333;
  font-weight: bold;
}

.b-table tbody tr:nth-child(even) {
  background-color: #f9f9f9;
}

.b-table tbody tr:hover {
  background-color: #f0f0f0;
  transition: background-color 0.3s ease-in-out;
}
</style>


