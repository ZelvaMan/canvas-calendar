<template>
  <div class="container">
    <div class="flex-container">
      <div class="flex-dates-collumn">
        <div style="height:25px; ">x</div>
        <div class="date">1.1</div>
        <div class="date">1.2</div>
        <div class="date">1.3</div>
        <div class="date">1.4</div>
        <div class="date">1.5</div>
      </div>
      <div class="flex-possision-collumn">
        <div class="resource-name">pizza</div>

        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
      </div>
      <PossisionCollumn
        possision="KP"
        startTime="10:00"
        endTime="20:00"
        :events="events2"
        beginningDate="	2020-08-10"
      ></PossisionCollumn>
      <div class="flex-possision-collumn">
        <div class="resource-name">bar</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
      </div>
      <div class="flex-possision-collumn">
        <div class="resource-name">servis</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
      </div>
      <div class="flex-possision-collumn">
        <div class="resource-name">uklid</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
      </div>
    </div>
  </div>
</template>

<script>
import PossisionCollumn from "./PossisionCollumn.vue";
export default {
  name: "WeekOverview",
  components: {
    PossisionCollumn
  },
  props: {
    startTime: String,
    endTime: String,
    events: Array,
    weekDateStart: String
  },
  data() {
    return {
      events1: [{ start: "10:00", end: "20:00", color: "red" }],
      events2: [
        {
          start: "	2020-08-13 10:00 ",
          end: "14:00",
          color: "blue",
          resource: "A"
        },
        {
          start: " 	2020-08-13 15:00",
          end: "18:00",
          color: "blue",
          resource: "A"
        },
        {
          start: " 	2020-08-13 10:00",
          end: "17:00",
          color: "red",
          resource: "B"
        },
        {
          start: "	2020-08-13 18:00",
          end: "20:00",
          color: "red",
          resource: "B"
        },
        {
          start: "	2020-08-14 10:00 ",
          end: "14:00",
          color: "blue",
          resource: "A"
        },
        {
          start: " 	2020-08-12 15:00",
          end: "18:00",
          color: "blue",
          resource: "A"
        },
        {
          start: " 	2020-08-15 10:00",
          end: "17:00",
          color: "red",
          resource: "B"
        },
        { start: "	2020-08-12 18:00", end: "20:00", color: "red", resource: "B" }
      ]
    };
  }
};
</script>

<style scoped>
.container {
  height: 100%;
}
.flex-container {
  display: flex;
  justify-content: flex-start;
  flex-direction: row;
  width: 100%;
  height: 100%;
  flex-wrap: nowrap;
}
.flex-dates-collumn {
  width: auto;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  font-size: 1.5rem;
  border: 1px black;
  height: 100%;
}
.flex-possision-collumn {
  width: auto;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  height: 100%;
  float: right;
  min-width: 40px;
}
.date {
  height: 20%;
  margin: 5px;
}
.resource-name {
  height: 25px;
  margin: 5px;
  min-width: 40px;
}

.line-container {
  margin: 5px;
  height: 20%;
  min-width: 40px;
  display: flex;
  flex-wrap: nowrap;
  flex-direction: row;
}

.red {
  background: red;
}
.blue {
  background: blue;
}
</style>
