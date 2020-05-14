<template>
  <div id="app" v-if="!loading">
    <div class="moon"></div>
    <header>
      <h1>{{currentFormattedMoonPhase}}</h1>
      <h2>Today is {{currentDate.format("MMMM Do, YYYY")}}</h2>
    </header>

    <footer>
      <h3>{{geocodedLocation}}</h3>
    </footer>
  </div>
</template>

<script>
const moment = require('moment');
const SunCalc = require('suncalc');

export default {
  name: 'App',
  data() {
    return {
      userLocation: {},
      geocodedLocation: '',
      loading: true,
    };
  },
  async created() {
    if ('geolocation' in navigator) {
      this.userLocation = await this.getUserLocation();
      this.geocodedLocation = await this.getGeocodedLocation(
        this.userLocation.coords.latitude,
        this.userLocation.coords.longitude,
      );
      this.loading = false;
    }
  },
  computed: {
    currentDate() {
      return moment();
    },
    currentFormattedMoonPhase() {
      let formattedMoonPhase;
      const phase = Math.round((SunCalc.getMoonIllumination(
        this.currentDate,
        this.userLocation.coords.latitude,
        this.userLocation.coords.longitude,
      ).phase + Number.EPSILON) * 100) / 100;

      switch (phase) {
        case 0: {
          formattedMoonPhase = 'The moon is a new moon today';
          break;
        }
        case 0.25: {
          formattedMoonPhase = 'The moon is one quarter today';
          break;
        }
        case 0.5: {
          formattedMoonPhase = 'The moon is full today';
          break;
        }
        case 0.75: {
          formattedMoonPhase = 'The moon is last quarter today';
          break;
        }
        default: {
          if (phase < 0 && phase > 0.25) {
            formattedMoonPhase = 'The moon is waxing crescent today';
          }
          if (phase > 0.25 && phase < 0.5) {
            formattedMoonPhase = 'The moon is waxing gibbous today';
          }
          if (phase > 0.5 && phase < 0.75) {
            formattedMoonPhase = 'The moon is waning gibbous today';
          }
          if (phase > 0.75) {
            formattedMoonPhase = 'The moon is waning crescent today';
          }
          break;
        }
      }

      return formattedMoonPhase;
    },
  },
  methods: {
    getUserLocation(options = {}) {
      return new Promise((resolve, reject) => {
        navigator.geolocation.getCurrentPosition(resolve, reject, options);
      });
    },
    async getGeocodedLocation(lat, lon) {
      const response = await fetch(`https://nominatim.openstreetmap.org/reverse?lat=${lat}&lon=${lon}&format=json`);
      const { address } = await response.json();
      return `${address.state}, ${address.country}`;
    },
  },
};
</script>

<style lang="scss">
@import "normalize-scss";
@include normalize();
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@300&family=Proza+Libre&display=swap");

:root {
  font-size: 1.6vw;
  @media screen and (min-width: 420px) {
    font-size: 1.2vw;
  }
  @media screen and (min-width: 768px) {
    font-size: 0.8vw;
  }
}

html,
body {
  width: 100%;
  height: 100%;
}

#app {
  display: grid;
  grid-template-rows: 2fr 2fr 1fr;
  @media screen and (min-width: 420px) {
    grid-template-rows: 3fr 2fr 1fr;
  }

  text-align: center;
  font-family: Open Sans, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: white;
  width: 100%;
  height: 100%;
  background: linear-gradient(255deg, #a4508b 0%, #5f0a87 100%) 0% 0%;
  text-shadow: 0 0 25px rgba(0, 0, 0, 0.3);
  opacity: 1;
}

#app .moon {
  width: 35em;
  height: 35em;
  border-radius: 50%;
  background-color: #a29595;
  justify-self: center;
  align-self: center;
  @media screen and (min-width: 420px) {
    width: 20em;
    height: 20em;
  }
}

#app h1,
h2,
h3 {
  font-family: Proza Libre;
}

#app h1 {
  font-size: 5em;
  margin: 0;
}

#app h2 {
  font-size: 3em;
}

#app h3 {
  font-size: 3.5em;
  text-align: left;
  @media screen and (min-width: 768px) {
    font-size: 2em;
  }
}

#app footer {
  justify-self: left;
  align-self: flex-end;
  margin-left: 2em;
}
</style>
