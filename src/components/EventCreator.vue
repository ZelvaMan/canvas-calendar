<template>
  <div class="container">
    <EventCreatorLine
      v-for="ri in resourceInfos"
      :key="ri.id"
      :events="getEventWithResource(ri.id)"
      :resourceInfo="ri"
      :daysOfWeek="daysOfWeek"
      :weekDateStart="startDate"
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
    resourceInfos: Array
  },
  watch: {
    events() {
      this.eventsByResource = this.groupByResource();
    }
  },
  mounted() {
    console.log(this.resourceInfo);
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
  }
};
</script>
<style scoped>
.container {
  width: 100%;
  border: 2px black dotted;
}
</style>