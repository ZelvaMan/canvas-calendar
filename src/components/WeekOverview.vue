<template>
  <div class="container">
    <div class="flex-container">
      <div class="flex-dates-collumb">
        <div style="height:25px; ">x</div>
        <div class="date">1.1</div>
        <div class="date">1.2</div>
        <div class="date">1.3</div>
        <div class="date">1.4</div>
        <div class="date">1.5</div>
      </div>
      <div class="flex-possision-collumb">
        <div class="resource-name">pizza</div>
        <div class="line-container">
          <resourceLine startTime="10:00" endTime="20:00" :events="this.events1"></resourceLine>
          <!-- <resourceLine startTime="10:00" endTime="20:00" :events="this.events2"></resourceLine> -->
        </div>
        <div class="line-container">
          <div class="line-container"></div>
        </div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
      </div>
      <div class="flex-possision-collumb">
        <div class="resource-name">KP</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
      </div>
      <div class="flex-possision-collumb">
        <div class="resource-name">bar</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
      </div>
      <div class="flex-possision-collumb">
        <div class="resource-name">servis</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
        <div class="line-container">x</div>
      </div>
      <div class="flex-possision-collumb">
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
import resourceLine from "./ResourceLine.vue";
export default {
  name: "WeekOverview",
  components: {
    resourceLine
  },
  props: {
    startTime: String,
    endTime: String,
    events: Array
  },
  data() {
    return {
      events1: [{ start: "10:00", end: "20:00", color: "red" }],
      events2: [
        { start: "10:00", end: "15:00", color: "black" },
        { start: "15:00", end: "20:00", color: "black" }
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
.flex-dates-collumb {
  width: auto;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  font-size: 1.5rem;
  border: 1px black;
  height: 100%;
}
.flex-possision-collumb {
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
