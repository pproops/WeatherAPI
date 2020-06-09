<template>
  <div id="app" :class="typeof weather.main != 'undefined'
   && weather.main.temp > 16 ? 'warm' : ''" >
    <main>

      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'" >
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <br> 
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
         <br>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="description">{{ weather.weather[0].description }}</div>
          <div id class="weatherIcons"> {{weather.weather[0].icon}}</div>
        </div>
      </div>
    </main>
  </div>
  
</template>


<script>

export default {
  
  name: 'app',
  
  data () {
    return {
      api_key: 'da87b2c9367ab0db30d061c2d93c4eae',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },

  methods: {
    fetchWeather (e) {
      if (e.key == "Enter"  ) {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    
    
    setResults (results) {
      this.weather = results;
    },
    

    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September",
      
       "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;

      
      
    }
  }
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
  background-image: url('./assets/first-bg.jpg'); 
  background-size: cover;
  background-position: bottom;
  transition: 0.3s;
}
#app.warm {
  background-image: url('./assets/sun-bg.jpg');

}
#app.cold {
  background-image: url('./assets/clouds-bg.jpg'); 
}
main {
  
  min-height: 100vh;
  padding: 35px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 50px;
}
.search-box .search-bar {
  display: block;
  width: 97%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #FFF;
  font-size: 36px;
  font-weight: 500;
  text-align: center;
  text-shadow: 2px 4px rgba(0, 0, 0, 0.25);
  
}
.location-box .date {
  color: rgb(207, 207, 207);
  font-size: 23px;
  font-weight: 300;
  font-style: Times;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: rgb(255, 248, 248);
  font-size: 98px;
  font-weight: 700;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 18px;
  margin: 30px 0px;
  box-shadow: 2px 3px rgba(0, 0, 0, 0.25);
}
.weather-box .description
{
  color: #FFF;
  font-size: 20px;
  font-style: italic;
}
.weather-box .weatherIcons
{
padding: 8px 12px;

color:blue;
font-size: 50px;
font-weight: 800;

}
.weather-box .weather {
  display: inline-block;
  padding: 12px 18px;
  color: rgb(255, 248, 248);
  font-size: 54px;
  font-weight: 700;
  font-style: initial;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 20px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>