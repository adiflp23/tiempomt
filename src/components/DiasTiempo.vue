<template>
<div class="days-tab text-center">
<div v-if="cargando" class="loading">Cargando...</div>
    <ul v-else class="p-0">
        <li v-for="day in forecast" :key="day.date" class="li_active">
            <div class="py-3"><img :src="day.iconUrl"></div>
            <div class="py-3">{{ getDayName(day.date) }}</div>
            <div class="py-3">{{ day.temperature }}&deg;</div>
        </li>
    </ul>
</div>

</template>

<script>
import axios from 'axios';
import moment from 'moment';

export default {
  props: {
    ciudadnombre: String,
  },
  data(){
    return{
        forecast: [],
        cargando: true,
        iconUrl: null
    }
  },
  mounted() {
    this.fetchTiempoDatos();
  },
  methods: {
    async fetchTiempoDatos(){
        const ciudad = this.ciudadnombre;

        await axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=${ciudad}&units=metric&appid=502f1c7b80f833988ac49eff956a42e3`).then(Response => {
            const forecastData = Response.data.list;
            const filteredeData = forecastData.map(item => {
                return {
                    date: moment(item.dt_txt.split(' ')[0]),
                    temperature: Math.round(item.main.temp),
                    description: item.weather[0].description,
                    iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`,
                };
            }).reduce((acc, item) => {
                if(!acc.some(day => day.date.isSame(item.date, 'day'))){
                    acc.push(item)
                }
                return acc;
            }, []).slice(1, 5);
               console.log(filteredeData, "funcionando"); 
               this.forecast = filteredeData;
               this.cargando = false;
        }).catch(error => {
            console.error("esta dando error el fetching", error);
            this.cargando = false
        })
    },
    getDayName(date){
        return date.format('ddd')
    }
  }
};
</script>

<style>
.days-tab {
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


</style>
