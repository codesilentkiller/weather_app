<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "a1a25207eb5f935a526f50c6c0cf616a",
      api_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter" && this.query.trim() !== "") {
        fetch(
          `${this.api_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((res) => res.json())
          .then(this.setWeather)
          .catch((err) => {
            this.weather = {}; // Clear previous data
            alert("Error fetching weather data");
          });
      }
    },
    setWeather(data) {
      this.weather = data;
    },
    // Use Intl.DateTimeFormat to format the date
    formatDate() {
      const options = {
        weekday: 'long',  // Monday, Tuesday, etc.
        day: 'numeric',   // 5
        month: 'long',    // January, February, etc.
        year: 'numeric',  // 2025
      };
      return new Date().toLocaleDateString('en-GB', options); // Format date
    }
  },
};
</script>

<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keyup.enter="fetchWeather"
        />
      </div>

      <!-- Weather Display -->
      <div class="weather-wrap" v-if="weather.main">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ formatDate() }}</div> <!-- Display the formatted date -->
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather-box">{{ weather.weather[0].main }}</div>
        </div>
      </div>

      <!-- Error Message -->
      <div v-else-if="weather.cod === '404'" class="error-message">
        <p>City not found. Please try again!</p>
      </div>
    </main>
  </div>
</template>



<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-attachment: cover;
  background-position: bottom;
  background-repeat: no-repeat;
  transition: 0.4s ease;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75)),
    url("./assets/cold-bg.jpg");
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 20px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
}

.search-box .search-bar::focus {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.5);
}

.location-box .location,
.location-box .date,
.weather-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #fff;
  margin-top: 30px;
  font-size: 34px;
  font-weight: 500;
  text-align: center;
  text-shadow: 2px 3px rgba(0, 0, 0, 0.35);
}

.weather-box {
  font-style: italic;
}

.temp {
  display: flex;
  box-shadow: 0px 0px 14px rgba(0, 0, 0, 0.25);
  width: 100%;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #fff;
  margin-top: 30px;
  font-size: 100px;
  font-weight: 800;
  text-align: center;
  text-shadow: 1px 1px rgba(0, 0, 0, 0.15);
}

.error-message {
  color: red;
  font-size: 18px;
  font-weight: bold;
  text-align: center;
  margin-top: 20px;
}
</style>
