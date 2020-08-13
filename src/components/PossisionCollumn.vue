<template>
  <div class="flex-possision-collumn">
    <div class="resource-name">{{this.possision}}</div>
    <LineContainer :startTime="startTime" :endTime="endTime"></LineContainer>
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
        var d;
        if ((d = this.haveDate(byDate, cdate) != undefined)) {
          console.log("havedate tru result");
          console.log(this.haveDate(byDate, cdate));
          result.push(d);
        } else {
          result.push({ date: cdate, events: [] });
        }
        console.log(d);
        console.log(cdate);
        cdate = moment(cdate)
          .add(1, "d")
          .format("MM.DD.");
      }
      console.log(events);
      return result;
    }
  },
  methods: {
    haveDate(array, date) {
      var tdate = moment(date).format("MM.DD.");
      var r = undefined;
      array.forEach(d => {
        if (d.date == tdate) {
          console.log("have date true");
          console.log(d);
          r = d;
          return;
        }
      });
      return r;
    }
  },
  mounted() {
    console.log(this.eventsGroupedByDay);
  }
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
  min-width: 40px;
}
</style>