<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <MovieListing />
  </div>
</template>

<script>
import MovieListing from "./components/organisms/MovieListing";

export default {
  name: "app",
  components: {
    MovieListing
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
