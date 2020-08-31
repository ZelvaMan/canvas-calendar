<template>
  <div id="app">
    <div class="flex-container">
      <WeekOverview
          :startTime="startTime"
          :endTime="endTime"
          :events="events"
          :startDate="date"
          :possisions="getPossisions"
          daysOfWeek="7"
      />
    
    <div class="right-container">
      <div class="info">
        <div class="vertical">
        <h1>WeekOverview</h1>
        <h3>
          <a href="https://github.com/ZelvaMan/canvas-calendar">Git hub</a>
        </h3>

        <div class="buttons">
        <button class="btn" v-on:click="removeWeek()">previous week</button>
        <button class="btn" v-on:click="addWeek()">next week</button>
        </div>
        </div>

              <div class="card collumns">
        <p>
          Barvy jmen
          <br />odpovidaji
          <br />barvam car
        </p>
        <div style="margin-left: 20px">
          <h3>zkratky</h3>
          <p>
            f == FOH
            <br />u == Uklid
            <br />b == BOH
            <br />cl == zaviracka
            <br />
          </p>
        </div>
        </div>
         
        
    </div>
     <EventCreator
          daysOfWeek="7"
          v-on:input="addEvent"
          :startTime="startTime"
          :endTime="endTime"
          :events="events"
          :resourceInfos="EventCreatorData.resourceInfo"
          :startDate="date"
        ></EventCreator>
    </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import WeekOverview from "./components/WeekOverview.vue";
import EventCreator from "./components/EventCreator";
import {
  dummyEventsForEventCreator,
  dummyResourceInfoForEventCreator,
  possisionList,
} from "./data/DummyData.js";
export default {
  name: "App",
  components: {
    WeekOverview,
    EventCreator,
  },
  data() {
    return {
      EventCreatorData: {
        events: dummyEventsForEventCreator,
        resourceInfo: dummyResourceInfoForEventCreator,
      },
      eventCreatorEvents: [],
      dummyEventsBool: true,
      possisions: possisionList,
      events: [],
      startTime: "8:00",
      endTime: "22:00",
      date: "2020/08/10",
    };
  },
  methods: {
    addEvent(events) {
      console.log(events);
      this.events = events;
    },
    removeWeek() {
      this.date = moment(this.date, "YYYY/MM/DD")
        .add(-7, "d")
        .format("YYYY/MM/DD");
    },
    addWeek() {
      this.date = moment(this.date, "YYYY/MM/DD")
        .add(7, "d")
        .format("YYYY/MM/DD");
    },
  },
  computed: {
    getPossisions() {
      var p = this.possisions;

      console.log(" get possisions:");
      console.log(p);
      return p;
    },
  },
  mounted() {
    console.log(this.data);
    moment.locale("cs");
    this.events = this.EventCreatorData.events;
  },
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
  margin-top: 25px;
}
.info {
  flex-direction: collumn;
  display: flex;
}
.btn {
  display: inline-block;
  margin-bottom: 0;
  font-weight: 400;
  width:50%;
  text-align: center;
  white-space: nowrap;
  vertical-align: middle;
  -ms-touch-action: manipulation;
  touch-action: manipulation;
  cursor: pointer;
  background-image: none;
  border: 1px solid black;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 1.42857143;
  border-radius: 4px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.btn.focus,
.btn:focus,
.btn:hover {
  color: #333;
  text-decoration: none;
}
.card {
  /* Add shadows to create the "card" effect */
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  padding: 15px;
  margin-left: 45px;
}
.collumns {
  display: flex;
  flex-direction: row;
}
/* On mouse-over, add a deeper shadow */
.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}
.right-container{
  display:flex;
  flex-direction: column;
  height: 100%;
}
.vertical{
  flex-direction: column;
  display: flex;
}
.buttons{
  flex-direction: row;
  display:flex;
}
</style>
