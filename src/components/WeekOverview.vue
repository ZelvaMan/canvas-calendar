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

        <b class="date">{{moment(weekDateStart).add(i -1, 'd').format("DD.MM.")}}</b>
        <br />
        <p class="endtime-text">{{endTime}}</p>
      </div>
    </div>
    <PossisionCollumn
      v-for="e in eventsGroupByPossision"
      :key="e.possision"
      :possision="e.possision"
      startTime="10:00"
      endTime="20:00"
      :events="e.events"
      beginningDate="	2020-08-10"
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
    weekDateStart: String
  },
  computed: {
    eventsGroupByPossision() {
      var result = this.$_.chain(this.events)
        // Group the elements of Array based on `color` property
        .groupBy("possision")
        // `key` is group's name (color), `value` is the array of objects
        .map((value, key) => ({ possision: key, events: value }))
        .value();
      console.log("grouped by possision");
      console.log(result);
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
.reset-this {
  animation: none;
  animation-delay: 0;
  animation-direction: normal;
  animation-duration: 0;
  animation-fill-mode: none;
  animation-iteration-count: 1;
  animation-name: none;
  animation-play-state: running;
  animation-timing-function: ease;
  backface-visibility: visible;
  background: 0;
  background-attachment: scroll;
  background-clip: border-box;
  background-color: transparent;
  background-image: none;
  background-origin: padding-box;
  background-position: 0 0;
  background-position-x: 0;
  background-position-y: 0;
  background-repeat: repeat;
  background-size: auto auto;
  border: 0;
  border-style: none;
  border-width: medium;
  border-color: inherit;
  border-bottom: 0;
  border-bottom-color: inherit;
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
  border-bottom-style: none;
  border-bottom-width: medium;
  border-collapse: separate;
  border-image: none;
  border-left: 0;
  border-left-color: inherit;
  border-left-style: none;
  border-left-width: medium;
  border-radius: 0;
  border-right: 0;
  border-right-color: inherit;
  border-right-style: none;
  border-right-width: medium;
  border-spacing: 0;
  border-top: 0;
  border-top-color: inherit;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
  border-top-style: none;
  border-top-width: medium;
  bottom: auto;
  box-shadow: none;
  box-sizing: content-box;
  caption-side: top;
  clear: none;
  clip: auto;
  color: inherit;
  columns: auto;
  column-count: auto;
  column-fill: balance;
  column-gap: normal;
  column-rule: medium none currentColor;
  column-rule-color: currentColor;
  column-rule-style: none;
  column-rule-width: none;
  column-span: 1;
  column-width: auto;
  content: normal;
  counter-increment: none;
  counter-reset: none;
  cursor: auto;
  direction: ltr;
  display: inline;
  empty-cells: show;
  float: none;
  font: normal;
  font-family: inherit;
  font-size: medium;
  font-style: normal;
  font-variant: normal;
  font-weight: normal;
  height: auto;
  hyphens: none;
  left: auto;
  letter-spacing: normal;
  line-height: normal;
  list-style: none;
  list-style-image: none;
  list-style-position: outside;
  list-style-type: disc;
  margin: 0;
  margin-bottom: 0;
  margin-left: 0;
  margin-right: 0;
  margin-top: 0;
  max-height: none;
  max-width: none;
  min-height: 0;
  min-width: 0;
  opacity: 1;
  orphans: 0;
  outline: 0;
  outline-color: invert;
  outline-style: none;
  outline-width: medium;
  overflow: visible;
  overflow-x: visible;
  overflow-y: visible;
  padding: 0;
  padding-bottom: 0;
  padding-left: 0;
  padding-right: 0;
  padding-top: 0;
  page-break-after: auto;
  page-break-before: auto;
  page-break-inside: auto;
  perspective: none;
  perspective-origin: 50% 50%;
  position: static;
  /* May need to alter quotes for different locales (e.g fr) */
  quotes: "\201C""\201D""\2018""\2019";
  right: auto;
  tab-size: 8;
  table-layout: auto;
  text-align: inherit;
  text-align-last: auto;
  text-decoration: none;
  text-decoration-color: inherit;
  text-decoration-line: none;
  text-decoration-style: solid;
  text-indent: 0;
  text-shadow: none;
  text-transform: none;
  top: auto;
  transform: none;
  transform-style: flat;
  transition: none;
  transition-delay: 0s;
  transition-duration: 0s;
  transition-property: none;
  transition-timing-function: ease;
  unicode-bidi: normal;
  vertical-align: baseline;
  visibility: visible;
  white-space: normal;
  widows: 0;
  width: auto;
  word-spacing: normal;
  z-index: auto;
  /* basic modern patch */
  all: initial;
  all: unset;
}
</style>
