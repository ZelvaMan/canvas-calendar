<template>
  <div class="flex-possision-collumn">
    <div class="resource-name">{{this.possision}}</div>
    <LineContainer
      :startTime="startTime"
      :endTime="endTime"
      v-for="date in eventsGroupedByDay"
      v-bind:key="date.date"
      :events="date.events"
    ></LineContainer>
  </div>
</template>
<script>
import _ from "lodash";
import Vue from "vue";
import moment from "moment";
Object.defineProperty(Vue.prototype, "$_", { value: _ });
import LineContainer from "./LineContainer.vue";
export default {
  name: "PossisionCollumn",
  components: {
    LineContainer
  },
  props: {
    startTime: String,
    endTime: String,
    possision: String,
    //!!!!!!!!!only for that specific week
    events: Array,
    beginningDate: String
  },
  computed: {
    eventsWDate() {
      var events = this.events;
      events.forEach(e => {
        e.date = moment(e.start).format("MM.DD.");
      });
      return events;
    },
    eventsGroupedByDay() {
      var result = [];
      var events = this.eventsWDate;
      var byDate = this.$_.chain(events)
        // Group the elements of Array based on `color` property
        .groupBy("date")
        // `key` is group's name (color), `value` is the array of objects
        .map((value, key) => ({ date: key, events: value }))
        .value();
      var cdate = moment(this.beginningDate).format("MM.DD.");
      for (let i = 0; i < 5; i++) {
        var d = this.haveDate(byDate, cdate);
        if (d != undefined) {
          result.push(d);
        } else {
          result.push({ date: cdate, events: [] });
        }
        cdate = moment(cdate)
          .add(1, "d")
          .format("MM.DD.");
      }
      return result;
    }
  },
  methods: {
    haveDate(array, date) {
      var tdate = moment(date).format("MM.DD.");
      var r = undefined;
      array.forEach(d => {
        if (d.date == tdate) {
          r = d;
          return;
        }
      });
      return r;
    }
  },
  mounted() {}
};
</script>
<style scoped>
.flex-possision-collumn {
  width: auto;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  height: 100%;
  float: right;

  border-left: 2px solid rgb(0, 0, 0, 0.4);
  min-width: 40px;
}
.resource-name {
  height: 25px;
  margin: 5px;
  min-width: 40px;
}
</style>