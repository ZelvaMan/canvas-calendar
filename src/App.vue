<template>
  <div id="app">
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
      events: eventsData,
      events2: [
        {
          start: "	2020-08-13 10:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "Pizza"
        },
        {
          start: " 	2020-08-13 15:00",
          end: "18:00",
          color: "blue",
          resource: "A",
          possision: "Pizza"
        },
        {
          start: " 	2020-08-10 10:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Pizza"
        },
        {
          start: "	2020-08-13 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Pizza"
        },
        {
          start: "	2020-08-14 10:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "Pizza"
        },
        {
          start: " 	2020-08-12 15:00",
          end: "18:00",
          color: "green",
          resource: "C",
          possision: "Pizza"
        },
        {
          start: " 	2020-08-15 10:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Pizza"
        },
        {
          start: "	2020-08-12 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Pizza"
        },
        {
          start: "	2020-08-13 10:00 ",
          end: "14:00",
          color: "green",
          resource: "C",
          possision: "Pizza"
        },
        {
          start: " 	2020-08-10 15:00",
          end: "18:00",
          color: "blue",
          resource: "A",
          possision: "Pizza"
        },
        {
          start: " 	2020-08-13 10:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Pizza"
        },
        {
          start: "	2020-08-11 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Pizza"
        },
        {
          start: "	2020-08-14 10:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "Pizza"
        },
        {
          start: " 	2020-08-12 15:00",
          end: "18:00",
          color: "green",
          resource: "C",
          possision: "Pizza"
        },
        {
          start: " 	2020-08-10 10:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Pizza"
        },
        {
          start: "	2020-08-11 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "KP"
        },
        {
          start: " 	2020-08-11 10:00",
          end: "20:00",
          color: "purple",
          resource: "D",
          possision: "KP"
        },
        {
          start: "  2020-08-13 12:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "KP"
        },
        {
          start: "  2020-08-13 15:00",
          end: "18:00",
          color: "blue",
          resource: "A",
          possision: "KP"
        },
        {
          start: "  2020-08-12 12:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "KP"
        },
        {
          start: "  2020-08-13 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "KP"
        },
        {
          start: "  2020-08-14 12:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "KP"
        },
        {
          start: "  2020-08-14 15:00",
          end: "18:00",
          color: "green",
          resource: "C",
          possision: "KP"
        },
        {
          start: "  2020-08-15 12:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "KP"
        },
        {
          start: "  2020-08-14 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "KP"
        },
        {
          start: "  2020-08-13 12:00 ",
          end: "14:00",
          color: "green",
          resource: "C",
          possision: "KP"
        },
        {
          start: "  2020-08-12 15:00",
          end: "18:00",
          color: "blue",
          resource: "A",
          possision: "KP"
        },
        {
          start: "  2020-08-13 12:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "KP"
        },
        {
          start: "  2020-08-14 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "KP"
        },
        {
          start: "  2020-08-11 12:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "KP"
        },
        {
          start: "  2020-08-14 15:00",
          end: "18:00",
          color: "green",
          resource: "C",
          possision: "KP"
        },
        {
          start: "  2020-08-12 12:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "KP"
        },
        {
          start: "  2020-08-11 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "KP"
        },
        {
          start: "  2020-08-11 12:00",
          end: "20:00",
          color: "purple",
          resource: "D",
          possision: "KP"
        },
        {
          start: "	2020-08-13 10:00 ",
          end: "19:00",
          color: "blue",
          resource: "A",
          possision: "Bar"
        },
        {
          start: " 	2020-08-13 15:00",
          end: "18:00",
          color: "blue",
          resource: "A",
          possision: "Bar"
        },
        {
          start: " 	2020-08-10 10:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Bar"
        },
        {
          start: "	2020-08-13 18:00",
          end: "19:00",
          color: "red",
          resource: "B",
          possision: "Bar"
        },
        {
          start: "	2020-08-14 10:00 ",
          end: "18:00",
          color: "blue",
          resource: "A",
          possision: "Bar"
        },
        {
          start: " 	2020-08-12 15:00",
          end: "18:00",
          color: "green",
          resource: "C",
          possision: "Bar"
        },
        {
          start: " 	2020-08-15 10:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Bar"
        },
        {
          start: "	2020-08-12 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Bar"
        },
        {
          start: "	2020-08-13 10:00 ",
          end: "14:00",
          color: "green",
          resource: "C",
          possision: "Bar"
        },
        {
          start: " 	2020-08-10 15:00",
          end: "18:00",
          color: "blue",
          resource: "A",
          possision: "Bar"
        },
        {
          start: " 	2020-08-13 10:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Bar"
        },
        {
          start: "	2020-08-11 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Bar"
        },
        {
          start: "	2020-08-14 10:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "Bar"
        },
        {
          start: " 	2020-08-12 15:00",
          end: "18:00",
          color: "green",
          resource: "C",
          possision: "Bar"
        },
        {
          start: " 	2020-08-10 10:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Bar"
        },
        {
          start: "	2020-08-11 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Bar"
        },
        {
          start: " 	2020-08-11 10:00",
          end: "20:00",
          color: "purple",
          resource: "D",
          possision: "Servise"
        },
        {
          start: "  2020-08-13 12:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "Servise"
        },
        {
          start: "  2020-08-13 15:00",
          end: "18:00",
          color: "blue",
          resource: "A",
          possision: "Servise"
        },
        {
          start: "  2020-08-12 12:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Servise"
        },
        {
          start: "  2020-08-13 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Servise"
        },
        {
          start: "  2020-08-14 12:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "Servise"
        },
        {
          start: "  2020-08-14 15:00",
          end: "18:00",
          color: "green",
          resource: "C",
          possision: "Servise"
        },
        {
          start: "  2020-08-15 12:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Servise"
        },
        {
          start: "  2020-08-14 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Servise"
        },
        {
          start: "  2020-08-13 12:00 ",
          end: "14:00",
          color: "green",
          resource: "C",
          possision: "Servise"
        },
        {
          start: "  2020-08-12 15:00",
          end: "18:00",
          color: "blue",
          resource: "A",
          possision: "Servise"
        },
        {
          start: "  2020-08-13 12:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Servise"
        },
        {
          start: "  2020-08-14 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Servise"
        },
        {
          start: "  2020-08-11 12:00 ",
          end: "14:00",
          color: "blue",
          resource: "A",
          possision: "Servise"
        },
        {
          start: "  2020-08-14 15:00",
          end: "18:00",
          color: "green",
          resource: "C",
          possision: "Servise"
        },
        {
          start: "  2020-08-12 12:00",
          end: "17:00",
          color: "red",
          resource: "B",
          possision: "Servise"
        },
        {
          start: "  2020-08-11 18:00",
          end: "20:00",
          color: "red",
          resource: "B",
          possision: "Servise"
        },
        {
          start: "  2020-08-11 12:00",
          end: "20:00",
          color: "purple",
          resource: "D",
          possision: "Servise"
        }
      ]
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
