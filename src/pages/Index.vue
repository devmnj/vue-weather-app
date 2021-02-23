<template>
  <q-page class="flex column" :class="bgClass">
     <div class="col q-pt-lg q-px-md">
        <q-input
          bottom-slots
          v-model="search"
          placeholder="Search Text"
          dark
          @keyup.enter="getWeatherBySearch"
          borderless
          >
          <template
          v-slot:before>
          <q-icon
          name="my_location"
          @click="getMylocation"
          />
          </template>
           
          <template
          v-slot:append>
          <q-btn
          round
          dense
          flat
          icon="search"
          @click="getWeatherBySearch"
          
          />
          </template>
          </q-input>
     </div>
 <template v-if="weatherData">
   <div class="col text-white text-center">
    
      <div class="text-h4 text-weight-light">
       {{ weatherData.name  }}, {{weatherData.sys.country}}
      </div>
      <div class="text-h6 text-weight-light">
        {{ weatherData.weather[0].main}} 
      </div>
      <div class="text-h1 text-weight-thin q-my-lg relative-position">
        <span>{{weatherData.main.temp}}</span>
        <span class="text-h4 relative-position degree">
        &deg;C</span>
      </div> 
      <div class="text-h6 text-weight-light">
        Wind .. {{ weatherData.wind.speed}} 
      </div>
      <div class="col text-center">
      <img :src="`https://openweathermap.org/img/wn/${this.weatherData.weather[0].icon}@2x.png`"></img>
    </div>
     </div>
 </template>
   <template v-else>
 <div class="col column text-white text-center">
    
      <div class="col text-h4 text-width-thin">
        Know your<br/>Weather 
      </div>
       <q-btn color="col" flat @click="getMylocation">
       <q-icon left size="3em" name="my_location" />
       <div>Find My Location</div>
       </q-btn> 
      
    </div> 
    </template>
  


<div class="skyline">

</div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  search:'', 
  data(){
    return {
      weatherData: null,
      search: "",
      lat: null,
      lon: null,
      api_URL: 'https:\\api.openweathermap.org/data/2.5/weather',
      API_KEY: '7dd8e3d4373e50a9fcaeb1f1ec87a024'


      }
  },
  computed:{
    bgClass(){
      if (this.weatherData){
        if(this.weatherData.weather[0].icon.endsWith(`n`)){
          return 'bg-night'
        }
        else{return 'bg-day'}
      }
    }
  }
  ,
  methods:{
    getMylocation(){
      if (this.$q.platform.is.electron){

        this.$axios(`https://freegeoip.app/json`).
        then(response=>{
            this.lat = response.data.latitude;
            this.lon = response.data.longitude;
            this.getWeatherByCoords();
   })
      }
      else{

       navigator.geolocation.getCurrentPosition(position =>{
         console.log('position',position);
         this.lat = position.coords.latitude;
         this.lon= position.coords.longitude;
         this.getWeatherByCoords();

       } 
       )
    }}
    ,
    getWeatherBySearch(){
      this.$q.loading.show();
      this.$axios(`${this.api_URL}?q=${this.search}&appid=${this.API_KEY}&units=metric`).
   then(response=>{
   console.log(response.data);
   this.weatherData=response.data   
      this.$q.loading.hide();

   }).catch((error)=>{
      this.search= ''; 
      this.$q.loading.hide()
   })
    },
  getWeatherByCoords(){
      this.$q.loading.show();
   this.$axios(`${this.api_URL}?lat=${this.lat}&lon=${this.lon}&appid=${this.API_KEY}&units=metric`).
   then(response=>{
   console.log(response.data);
   this.weatherData=response.data 
      this.$q.loading.hide();

   })

  }

  }
 
}
</script>

<style lang="css">
.skyline{
  flex: 0 0 100px;
  background: url(../statics/skyline3.png);
  background-size: contain;
  background-position: bottom center;
}
.degree{
  top: -50px
}
.q-page{
  background: linear-gradient(to bottom, #2c3e50, #3498db);  
}
.bg-night{
 background: linear-gradient(to right, #e8cbc0, #636fa4);  
}

.bg-day{
background: linear-gradient(to right, #3a7bd5, #00d2ff); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

</style>
