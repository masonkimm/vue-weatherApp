<template>
  <img id="vue-logo" alt="Vue logo" src="./assets/logo.png" />

  <div
    id="app"
    :class="weather && convertTemp(weather.main.temp) > 70 ? 'warm' : ''"
  >
    <main>
      <div class="search-box">
        <form action="" @submit="onSubmit">
          <input
            v-model="query"
            v-on:keypress="fetchWeather"
            type="text"
            class="search-bar"
            placeholder="Search..."
          />
        </form>
      </div>
      <div class="weather-wrap" v-if="weather">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder().toDateString() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">
            {{ Math.round(convertTemp(weather.main.temp)) }}°F
          </div>
          <!-- <div class="temp">{{ Math.round(weather.main.temp) }}°C</div> -->

          <div class="weather">
            {{ weather.weather[0].main }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: 'b328ccab8d372c776afbedb2b4434e8c',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: null,
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      fetch(`${this.url_base}weather?q=${this.query}&APPID=${this.api_key}`)
        .then((res) => {
          if (res.ok) {
            return res.json();
          } else {
            throw new Error('Something went wrong');
          }
        })
        .then(this.setResults)
        .catch(() => {
          alert('Please check spelling');
        });
    },
    setResults(results) {
      // console.log(results);
      this.weather = results;
    },
    convertTemp(temp) {
      return (parseFloat(temp) - 273.15) * 1.8 + 32;
    },
    dateBuilder() {
      let newDate = new Date();
      return newDate;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-image: url('./assets/cold-bg.jpg');
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
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  border: none;
  outline: none;
  background: none;
  appearance: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0;
}
.location-box .location {
  color: #ffff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: rgb(73, 71, 71);
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
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgb(0, 0, 0, 0.25);
}
#vue-logo {
  position: fixed;
  right: 0;
  bottom: 0;
  height: 8vmin;
}
</style>
