<template>
  <div id="app">
    <h1>WeekOverview</h1>
    <h3>
      <a href="https://github.com/ZelvaMan/canvas-calendar">Git hub</a>
    </h3>
    <div class="flex-container">
      <WeekOverview startTime="10:00" endTime="20:00" :events="events" weekDateStart="2020-08-10" />
      <EventCreator daysOfWeek="5" v-on:input="addEvent"></EventCreator>
    </div>
  </div>
</template>

<script>
import WeekOverview from "./components/WeekOverview.vue";
import EventCreator from "./components/EventCreator";
import { eventsData } from "./data/events.js";
export default {
  name: "App",
  components: {
    WeekOverview,
    EventCreator
  },
  data() {
    return {
      dummyEvents: eventsData,
      eventCreatorEvents: [],
      dummyEventsBool: true
    };
  },
  methods: {
    addEvent(event) {
      console.log(event);
      this.eventCreatorEvents = this.eventCreatorEvents.concat(event);
    }
  },
  computed: {
    events() {
      var events = [];
      if (this.dummyEventsBool) {
        events = this.dummyEvents.concat(this.eventCreatorEvents);
      } else {
        events = this.eventCreatorEvents;
      }
      return events;
    }
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
.flex-container {
  flex-direction: row;

  height: 100%;
  display: flex;
}
</style>
