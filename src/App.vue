<template>
  <div id="app">
    <WeekOverview />
  </div>
</template>

<script>
import WeekOverview from "./components/WeekOverview.vue";

export default {
  name: "App",
  components: {
    WeekOverview
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100%;
}
</style>
