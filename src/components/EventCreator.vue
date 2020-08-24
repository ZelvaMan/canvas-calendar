<template>
  <div class="container">
    <div class="dates-container" :style="colorBorderStyleString">
      <div :style="'width:' + getNameWidth.toString()  + 'rem;'"></div>
      <div
        class="date-container"
        v-for="n in parseInt(daysOfWeek)"
        :key="n"
        :style="'width:'+ 100/daysOfWeek + '%;'"
      >{{moment(startDate).add(n,"d").format("MM/DD")}}</div>
    </div>
    <EventCreatorLine
      v-for="ri in resourceInfos"
      :key="ri.id"
      :events="getEventWithResource(ri.id)"
      :resourceInfo="ri"
      :daysOfWeek="daysOfWeek"
      :weekDateStart="startDate"
      :possisions="possisions"
      :NameSize="getNameWidth"
      v-on:input="onInput"
    ></EventCreatorLine>
  </div>
</template>

<script>
import _ from "lodash";
import Vue from "vue";
//import moment from "moment";
Object.defineProperty(Vue.prototype, "$_", { value: _ });
import EventCreatorLine from "./EventCreatorLine";
export default {
  name: "EventCreator",
  components: {
    EventCreatorLine
  },
  data() {
    return {
      eventsByResource: this.groupByResource()
    };
  },
  props: {
    daysOfWeek: String,
    startDate: String,
    events: Array,
    resourceInfos: Array,
    possisions: Array
  },
  watch: {
    events() {
      this.eventsByResource = this.groupByResource();
    }
  },
  mounted() {
    console.log("possisions:");
    console.log(this.possisions);
  },
  methods: {
    onInput(e) {
      //need to fix emit so it will join with other resources
      this.$emit("input", this.eventsForEmit(e));
    },

    getResourceInfo(resource) {
      var res = null;
      this.resourceInfos.forEach(rs => {
        if (rs.id == resource) res = rs;
      });
      return res;
    },

    getEventWithResource(resource) {
      var result = [];
      this.eventsByResource.forEach(e => {
        if (e.resource == resource) result = e.events;
      });
      return result;
    },
    groupByResource() {
      var result = this.$_.chain(this.events)
        // Group the elements of Array based on `color` property
        .groupBy("resource")
        // `key` is group's name (color), `value` is the array of objects
        .map((value, key) => ({ resource: key, events: value }))
        .value();
      return result;
    },
    eventsForEmit(groupedResource) {
      var events = [];
      var concated = false;
      this.eventsByResource.forEach(r => {
        if (r.resource == groupedResource.resource) {
          //is resource curently emited
          concated = true;
          events = events.concat(groupedResource.events);
        } else {
          events = events.concat(r.events);
        }
      });
      if (!concated) {
        events = events.concat(groupedResource.events);
      }
      return events;
    }
  },
  computed: {
    getNameWidth() {
      var longest = this.resourceInfos.reduce(function(a, b) {
        return a.name.length > b.name.length ? a : b;
      });
      console.log("longest:");
      console.log(longest);
      return longest.name.length;
    }
  }
};
</script>
<style scoped>
.container {
  width: 100%;
}
.dates-container {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  margin: 10px;
  width: auto;
  border-bottom: 2px solid black;
}

.date-container {
  margin: 5px;
  font-size: 16px;
}
</style>