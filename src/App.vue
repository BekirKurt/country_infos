<template>
  <country-info></country-info>
</template>

<script>
import CountryInfo from "./components/CountryInfo.vue";

export default {
  name: "App",
  components: {
    "country-info": CountryInfo,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: azure;
  background-color: #3d5656;
}
</style>
