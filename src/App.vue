<template>
  <div id="app" class>
    <main class="min-h-screen p-4" :class="typeof weather.main !='undefined' && weather.main.temp > 20 ? 'warm' : ''">
      <div class="search-box mb-8 w-full">
        <input
          type="text"
          class="search-bar w-full block p-4 appearance-none border-none outline-none placeholder-green-400"
          placeholder="Enter City Name..."
          v-model="query"
          @keyup.enter="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box ">
          <div class="location text-center font-medium text-white">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date text-center italic font-light text-white">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box text-center">
          <div class="temp inline-block text-white font-black m-8 shadow-xl ">{{Math.round(weather.main.temp)}}°C</div>
          <div class="status text-white font-bold italic ">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "746b12553994b2ba868b7c195d663e34",
      url_base:'http://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{},
    };
  },
  methods:{
    fetchWeather(){
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`).then(res=>{
        return res.json();
      }).then(this.setResults)
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date()
      let days = ["Saturday","Sunday","Monday","Tuesday","Wednesday","Thursday","Friday"];
      let months = ["January","February","March","April","May","June","July","August","September","October","November","December"]
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
    }

  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
}
#app {
  background-size: cover;
  background-image: url("./assets/cold-background.jpg");
  background-position: bottom;
  transition: 0.4s;
}
#app .warm{
  background-image: url("./assets/warm-background.jpg");
  background-repeat: no-repeat;
  background-size:cover ;
}
main {
  background-image: linear-gradient(
    to bottom,
    rgba(79, 88, 128, 0.25),
    rgba(161, 38, 38, 0.56)
  );
}
.search-box .search-bar {
  color: #313131;
  font-size: 20px;

  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.6s;
  box-shadow: 0, 0, 16, rgba(0, 0, 0, 0.25);
}
.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 16px 0 16px 0;
  box-shadow: 0, 0, 16, rgba(0, 0, 0, 0.55);
}
.location-box .location{
  font-size: 36px;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-box .temp{
  font-size: 100px;
  padding: 10px 30px;
  text-shadow: rgba(0, 0, 0, 0.65);
  background: rgba(255, 255, 255, 0.25);
  border-radius: 15px;
}
.weather-box .status{
  font-size: 48px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
