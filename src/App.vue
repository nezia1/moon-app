<template>
  <div id="app">
    <div class="moon"></div>
    <header>
      <h1>The moon is full today</h1>
      <h2>Today is {{currentDate.format("MMMM Do, YYYY")}}</h2>
    </header>

    <footer>
      <h3>Seoul, South Korea</h3>
    </footer>
  </div>
</template>

<script>
const moment = require('moment');

export default {
  name: 'App',
  computed: {
    currentDate() {
      return moment();
    },
  },
};
</script>

<style lang="scss">
@import "normalize-scss";
@include normalize();
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@300&family=Proza+Libre&display=swap");

:root {
  font-size: 16px;
}

#app {
  display: grid;
  grid-template-rows: 3fr 2fr 1fr;
  text-align: center;
  font-family: Open Sans, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: white;
  width: 100vw;
  height: 100vh;
  background: linear-gradient(255deg, #a4508b 0%, #5f0a87 100%) 0% 0%;
  text-shadow: 0 0 25px rgba(0, 0, 0, 0.3);
  opacity: 1;
}
#app .moon {
  width: 20em;
  height: 20em;
  border-radius: 50%;
  background-color: #a29595;
  justify-self: center;
  align-self: center;
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
  font-size: 2em;
}

#app footer {
  justify-self: left;
  align-self: flex-end;
  margin-left: 2em;
}
</style>
