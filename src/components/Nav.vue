<template>
  <div class="card">
    <div ref="search" class="search">
      <input type="text" placeholder="your city" spellcheck="false" v-model="cityName" />
      <button @click="searchWeather"><img class="icon" src="./icons/tuvois.jpg" alt="search" /></button>
    </div>
    <div ref="w-icon" class="weather">
      <img :src="weatherIcon" class="weather-icon" />
      <h1 ref="temps" class="temp">22°c</h1>
      <h2 ref="cityNameResult" class="city">New York</h2>
      <div class="details">
        <div class="col">
          <img src="./icons/humidity.jpg" class="subi" />
          <div>
            <p ref="humy" class="humidity">%</p>
            <p>Humidity</p>
          </div>
        </div>
        <div class="col">
          <img src="./icons/wind.jpg" class="subi" />
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
      weatherIcon: "./src/components/icons/claude.jpg", // default icon
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
      if(data.weather[0].main === "Clouds"){
        this.weatherIcon = "./src/components/icons/claude.jpg";
      } else {
        this.weatherIcon = "./src/components/sun.jpg";
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
  width: 170px;
}
.weather h1 {
  font-size: 80px;
}
.weather h1 {
  font-size: 45px;
  margin-top: -10px;
}
.details {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  margin-top: 50px;
}
.subi {
  width: 40px;
}
</style>
