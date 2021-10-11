<template>
  <div id="app" :class="weather.main && weather.main.temp > 16 ? 'warm' : ''"  >
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search ..."
          v-model="query"
          @keypress.enter="fetchWeather"
          />
      </div>

      <div class="weather-wrap" v-if="weather.main">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ new Date().toLocaleDateString() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: '556bf2c32444a155a06e608acb3d0efa',
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {}
    }
      
  },
  methods: { 
    fetchWeather(e) {
      if(e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },

    setResults(results) {
      this.weather = results;
      console.log(results);
    },

    dateBuilder() {

    }
    
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  background-repeat: no-repeat;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding:  25px;
  /* width: 100%; */
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 1rem;
  border: none;
  outline: none;
  border-radius: 0 13px 0 13px;
  font-size: 1.3rem;
  color: rgb(46, 53, 58);
  background: none;
  appearance: none;
  background-color: rgba(255,255,255,0.5);
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
}
.search-box .search-bar:focus {
  background-color: rgba(255,255,255,0.8);
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  border-radius: 16px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather-box .weather {
  color: #FFF;
  font-size: 50px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
} 
</style>
