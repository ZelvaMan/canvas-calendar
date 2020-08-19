<template>
  <div id="app">
    <h1>WeekOverview</h1>
    <h3>
      <a href="https://github.com/ZelvaMan/canvas-calendar">Git hub</a>
    </h3>
    <div class="flex-container">
      <WeekOverview
        :startTime="startTime"
        :endTime="endTime"
        :events="events"
        weekDateStart="2020-08-10"
      />
      <EventCreator
        daysOfWeek="5"
        v-on:input="addEvent"
        :events="events"
        :resourceInfos="EventCreatorData.resourceInfo"
        startDate="2020-08-10"
      ></EventCreator>
    </div>
  </div>
</template>

<script>
import WeekOverview from "./components/WeekOverview.vue";
import EventCreator from "./components/EventCreator";
import {
  dummyEventsForRenderTest,
  dummyEventsForEventCreator,
  dummyResourceInfoForEventCreator
} from "./data/DummyData.js";
export default {
  name: "App",
  components: {
    WeekOverview,
    EventCreator
  },
  data() {
    return {
      EventsForRenderTest: dummyEventsForRenderTest,
      EventCreatorData: {
        events: dummyEventsForEventCreator,
        resourceInfo: dummyResourceInfoForEventCreator
      },
      eventCreatorEvents: [],
      dummyEventsBool: true,
      events: [],
      startTime: "8:00",
      endTime: "22:00"
    };
  },
  methods: {
    addEvent(events) {
      console.log(events);
      this.events = events;
    }
  },
  mounted() {
    console.log(this.data);
    this.events = this.EventCreatorData.events;
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
