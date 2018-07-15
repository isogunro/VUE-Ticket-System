<template>
  <div id="app">
  <div>
    <isaac/>
   </div>
  </div>
</template>

<script>
import isaac from "./components/isaac";

export default {
  name: "App",
  components: {
    isaac
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
