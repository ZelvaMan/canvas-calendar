<template>
  <div class="flex-container">
    <div class="flex-dates-collumn">
      <a href="http://keenmate.com/">
        <div class="logo-container">
          <img src="../img/KeenmateLogo.png" style="height:25px; width:25px;" />

          <b style="font-size: 0.7rem;">
            KEEN|
            <br />|MATE
          </b>
        </div>
      </a>
      <div class="date-container" v-for="i in 5" :key="i">
        <p class="starttime-text">{{startTime}}</p>
        <br />

        <b class="date">{{moment(StartDate,"YYYY/MM/DD").add(i -1, 'd').format("DD.MM.")}}</b>
        <br />
        <p class="endtime-text">{{endTime}}</p>
      </div>
    </div>
    <PossisionCollumn
      v-for="e in eventsGroupByPossision"
      :key="e.possision"
      :possision="e.possision"
      :startTime="startTime"
      :endTime="endTime"
      :events="e.events"
      beginningDate="	2020/08/10"
      :daysOfWeek="daysOfWeek"
    ></PossisionCollumn>
  </div>
</template>

<script>
import _ from "lodash";
import Vue from "vue";
//import moment from "moment";
Object.defineProperty(Vue.prototype, "$_", { value: _ });
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
    //date on which you want to start render week
    StartDate: String,
    daysOfWeek: String
  },
  computed: {
    //group events by possision
    eventsGroupByPossision() {
      var result = this.$_.chain(this.events)
        .groupBy("possision")
        .map((value, key) => ({ possision: key, events: value }))
        .value();
      return result;
    }
  }
};
</script>

<style scoped>
.flex-container {
  display: flex;
  justify-content: flex-start;
  flex-direction: row;
  width: auto;
  height: 100%;
  flex-wrap: nowrap;
  border-style: solid;
  border-width: 2px;
  margin-right: 10px;
}
.flex-dates-collumn {
  width: auto;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  font-size: 1.2rem;
  height: 100%;
}
.date::after {
  content: "";
  position: absolute;
  z-index: -1;
  left: 0;
  right: 0;
  top: 50%;
  border-top: 1px solid rgb(0, 0, 0, 0.3);
}
.date-container {
  height: 20%;
  padding: 5px;
  border-top: 2px solid rgb(0, 0, 0, 0.4);
  position: relative;
  display: grid;
  grid-template-rows: 1rem auto 1rem;
}
.date {
  grid-row: 2;
}
.starttime-text {
  font-size: 0.5rem;
  margin-block-start: 0rem;
  grid-row: 1;
}
.endtime-text {
  font-size: 0.5rem;
  align-items: end;
  vertical-align: bottom;
  margin-block-end: 0rem;
  grid-row: 5;
}
.logo-container {
  display: flex;
  flex-flow: row;
  color: black;
  text-decoration: none;
  flex-wrap: nowrap;
  height: 30px;
}
</style>
