<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input
        bottom-slots
        v-model="search"
        placeholder="Search"
        dark
        borderless
      >
        <template v-slot:before>
          <q-icon @click="getLocation" name="my_location" />
        </template>
        <template v-slot:append>
          <q-btn round dense flat icon="search" />
        </template>
      </q-input>
    </div>
    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">Tehran</div>
        <div class="text-h6 text-weight-light">Rain</div>
        <div class="text-h1 text-weight-thin q-my-md relative-position">
          <span>8</span>
          <span class="text-h4 relative-position degree">&deg;</span>
        </div>
      </div>
      <div class="col text-center">
        <img src="" alt="" />
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
    };
  },
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition(successCallback, errorCallback, {
        timeout: 10000,
      });
      console.log("successCallback");
      // navigator.geolocation.getCurrentPosition((position) => {
      //   console.log("position:", position);
      //   this.lat = position.coords.latitude;
      //   this.lon = position.coords.longitude;
      // });
    },
  },
};
</script>
<style lang="sass">
.q-page
  background: linear-gradient(to bottom, #136a8a, #267871)
.degree
  top: -44px
.skyline
  flex: 0 0 100px
  background: url(../../public/skyline.png)
  background-size: contain
  background-position: center bottom
</style>
