<template>
  <div id="app">
    <div class="info">
      <div style="width:60%;">
        <h1>WeekOverview</h1>
        <h3>
          <a href="https://github.com/ZelvaMan/canvas-calendar">Git hub</a>
        </h3>
      </div>
      <div class="card">
        <h3>zkratky</h3>
        <p>
          p == Pizza
          <br />u == Uklid
          <br />b == Bar
          <br />s == Servis
          <br />k == Kuchar
          <br />
        </p>
      </div>
    </div>
    <div class="flex-container">
      <WeekOverview
        :startTime="startTime"
        :endTime="endTime"
        :events="events"
        StartDate="2020/08/10"
        :possisions="getPossisions"
        daysOfWeek="5"
      />
      <EventCreator
        daysOfWeek="5"
        v-on:input="addEvent"
        :events="events"
        :resourceInfos="EventCreatorData.resourceInfo"
        startDate="2020/08/10"
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
  dummyResourceInfoForEventCreator,
  possisionList
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
      possisions: possisionList,
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
  computed: {
    getPossisions() {
      var p = this.possisions;

      console.log(" get possisions:");
      console.log(p);
      return p;
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
.info {
  flex-direction: collumn;
  display: flex;
}
.card {
  /* Add shadows to create the "card" effect */
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  width: 20%;
}

/* On mouse-over, add a deeper shadow */
.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}
</style>
