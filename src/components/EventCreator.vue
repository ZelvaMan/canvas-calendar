<template>
  <div class="container">
    <EventCreatorLine
      v-for="ri in resourceInfos"
      :key="ri.id"
      :events="getResourceInfo(ri.id)"
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
    return {};
  },
  props: {
    daysOfWeek: String,
    startDate: String,
    events: Array,
    resourceInfos: Array
  },
  mounted() {
    console.log(this.resourceInfo);
  },
  methods: {
    onInput(e) {
      this.$emit("input", e);
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
      this.events.forEach(e => {
        if (e.resource == resource) result.push(e);
      });

      return result;
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