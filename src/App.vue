<template>
  <div id="app">
    <Weather />
  </div>
</template>

<script>
import Weather from "./components/Weather.vue";

export default {
  name: "App",
  components: {
    Weather,
  },
};
</script>

<style>
body {
  background-color: #58bac9;
  font-family: sans-serif;
  margin: inherit;
}

#app {
  width: 100%;
  height: 100vh;
}
</style>
