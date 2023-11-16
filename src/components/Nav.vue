<template>
  <div class="card">
    <div ref="search" class="search">
      <input type="text" placeholder="Votre ville" spellcheck="false" v-model="cityName" />
      <button @click="searchWeather"><img class="icon" src="./icons/tuvois.jpg" alt="search" /></button>
    </div>
    <div ref="w-icon" class="weather">
      <img :src="weatherIcon" class="weather-icon" />
      <h1 ref="temps" class="temp">22°c</h1>
      <h2 ref="cityNameResult" class="city">New York</h2>
      <div class="details">
        <div class="col">
          <img src="../assets/droplet.png" class="cond-icon" />
          <div>
            <p ref="humy" class="humidity">%</p>
            <p>Humidity</p>
          </div>
        </div>
        <div class="col">
          <img src="../assets/wind.png" class="cond-icon" />
          <div>
            <p ref="nreaker" class="wind">50Km/H</p>
            <p>wind</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const apiKey = "ddc1e28203d38920f4278334d511c073";
const apiUrl = "https://api.openweathermap.org/data/2.5/weather?&units=metric&q=";

export default {
  data() {
    return {
      cityName: '',
      weatherIcon: "./src/assets/clouds.png",
    };
  },
  mounted() {
    this.checkWeather();
  },
  methods: {
  async checkWeather(city = 'Roubaix') {
    const response = await fetch(`${apiUrl}${city}&appid=${apiKey}`);
    const data = await response.json();
    console.log(data);
    this.$refs.cityNameResult.innerHTML = data.name;
    this.$refs.temps.innerHTML = Math.round(data.main.temp) + "°c";
    this.$refs.humy.innerHTML = data.main.humidity + "%";
    this.$refs.nreaker.innerHTML = data.wind.speed + "km/h";

    if (data.weather[0].main === "Clouds") {
      this.weatherIcon = "/src/assets/clouds.png";
    } else if (data.weather[0].main === "Clear") {
      this.weatherIcon = "../src/assets/sun.png";
    } else if (data.weather[0].main === "Rain") {
      this.weatherIcon = "./src/assets/rain.png";
    } else if (data.weather[0].main === "Snow") {
      this.weatherIcon = "./src/assets/snow.png";
    } else if (data.weather[0].main === "Thunderstorm") {
      this.weatherIcon = "../assets/thunderstorm.png";
    } else if (data.weather[0].main === "Drizzle") {
      this.weatherIcon = "/assets/drizzle.png";
    } else {
      this.weatherIcon = "./src/assets/default.png";
    }
  },
  searchWeather() {
    this.checkWeather(this.cityName);
  },
},

};
</script>

<style>
.card {
  background: linear-gradient(135deg, #00feba, #5b548a);
  padding: 40px 35px;
  border-radius: 30px;
}

.icon {
  width: 40px;
}

.search {
  display: flex;
  align-items: center;
}

.search input {
  border-radius: 30px;
}

.weather-icon {
  width: 40px;
}

.weather h1 {
  font-size: 80px;
}

.details {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  margin-top: 50px;
}

.cond-icon {
  width: 40px;
}

@media only screen and (min-width: 768px) {

  .card {
    width: 100%;
    max-width: 600px;
    margin: 0 auto; 
  }
}
</style>
