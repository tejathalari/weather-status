<template>
  <div id="app" :class="(typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : '')">
    <main>
      <div class="center-container">
        <div class="search-box">
          <input 
            type="text" 
            class="search-box" 
            placeholder="Search..."
            v-model="searchInput"
            @keypress="fetchWeather"
          />
          <button @click="fetchWeather" class="search-button">Forecast</button> 

        </div>
      </div>

      <div class="center-container">
        <div class="weather-now" v-if="(typeof weather.main != 'undefined')">
          <div class="location-box">
            <div class="location">{{ weather.name }}, {{ weather.sys.city }}</div>
            <div class="date">{{ dateBuilder() }}</div>
          </div>
          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}˚c</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      api_key: '61d29da8da970b7515baaa7743bb881b',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      searchInput: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
        if (e.key == "Enter") {
          fetch(`${this.url_base}weather?q=${this.searchInput}&units=metric&APPID=${this.api_key}`)
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
      let months = ["January", "February", "March", " April", "May", "June", "July",
      "August", "september", "october", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday",
      "Saturday"];

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

.center-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 20px;
  height: 10vh; /* Set to full viewport height */
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.PNG');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(52, 50, 50, 0.25), rgba(40, 39, 39, 0.75));
}

.search-box {
  width: 60%;
  margin-bottom: 30px;
  height: 30px;
  margin-left: 70px;
}

.search-button {
  background-color: #007bff; /* Change to your desired color */
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  padding: 5px 10px;
  font-size: 16px;
}

.weather-now {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  color: aliceblue;
  margin-top: 20px;
}

.location-box {
  font-size: 50px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin-right: 20px;
}

.date {
  font-size: 40px;
  font-weight: 300;
  font-style: italic;
  margin-right: 20px;
}

.temp {
  font-size: 40px;
  display: inline-block;
}

.weather {
  font-size: 30px;
  margin-left: 5px;
}

</style>
