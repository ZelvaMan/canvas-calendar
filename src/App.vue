<template>
  <div id="app">
    <h1>WeekOverview</h1>
    <h3>
      <a href="https://github.com/ZelvaMan/canvas-calendar">Git hub</a>
    </h3>
    <WeekOverview startTime="10:00" endTime="20:00" :events="events" weekDateStart="2020-08-10" />
  </div>
</template>

<script>
import WeekOverview from "./components/WeekOverview.vue";
import { eventsData } from "./data/events.js";
export default {
  name: "App",
  components: {
    WeekOverview
  },
  data() {
    return {
      events: eventsData
    };
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
