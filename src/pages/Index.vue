<template>
  <q-page class="flex column" :class="bgClass">
    <div class="col q-pt-lg q-px-md">
      <q-input
        bottom-slots
        @keyup.enter="getWeatherBySearch"
        v-model="search"
        placeholder="Search"
        dark
        borderless
      >
        <template v-slot:before>
          <q-icon @click="getLocation" name="my_location" />
        </template>
        <template v-slot:append>
          <q-btn @click="getWeatherBySearch" round dense flat icon="search" />
        </template>
      </q-input>
    </div>
    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">{{ weatherData.name }}</div>
        <div class="text-h6 text-weight-light">
          {{ weatherData.weather[0].main }}
        </div>
        <div class="text-h1 text-weight-thin q-my-md relative-position">
          <span>{{ Math.round(weatherData.main.temp) }}</span>
          <span class="text-h4 relative-position degree">&deg;C</span>
        </div>
      </div>
      <div class="col text-center">
        <img
          :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`"
        />
      </div>
    </template>
    <template v-else>
      <div class="col column text-center text-white">
        <div class="col text-h2 text-weight-thin">Quasar<br />Weather</div>
        <q-btn
          @click="getLocation"
          class="col"
          flat
          icon="my_location"
          label="Find My Location"
        />
      </div>
    </template>

    <div class="col skyline"></div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      search: "",
      weatherData: null,
      lat: null,
      lon: null,
      position: "",
      apiUrl: "https://api.openweathermap.org/data/2.5/weather",
      apiKey: "3f9b2209f190d9ab272ed26731fa3566",
    };
  },
  computed: {
    bgClass() {
      if (this.weatherData) {
        if (this.weatherData.weather[0].icon.endsWith("n")) {
          return "bg-night";
        } else {
          return "bg-day";
        }
      }
    },
  },
  methods: {
    async getLocation() {
      this.$q.loading.show();
      return new Promise((resolve, reject) => {
        if (!("geolocation" in navigator)) {
          reject(new Error("Geolocation is not available."));
        }

        navigator.geolocation.getCurrentPosition(
          (pos) => {
            resolve(pos);
            this.lat = pos.coords.latitude;
            this.lon = pos.coords.longitude;
            this.getWatherByCoords();
          },
          (err) => {
            reject(err);
          }
        );
      });
    },
    getWatherByCoords() {
      this.$q.loading.show();
      this.$axios(
        `${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`
      ).then((response) => {
        this.weatherData = response.data;
        this.$q.loading.hide();
      });
    },
    getWeatherBySearch() {
      this.$q.loading.show();
      console.log("getWeatherBySearch");
      this.$axios(
        `${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`
      ).then((response) => {
        this.weatherData = response.data;
        this.$q.loading.hide();
      });
    },
  },
};
</script>
<style lang="sass">
.q-page
  background: linear-gradient(to bottom, #136a8a, #267871)
  &.bg-night
    background: linear-gradient(to bottom, #232526, #414345)
  &.bg-day
    background: linear-gradient(to bottom, #00b4db, #0083b0)
.degree
  top: -44px
.skyline
  flex: 0 0 100px
  background: url(../../public/skyline.png)
  background-size: contain
  background-position: center bottom
</style>
