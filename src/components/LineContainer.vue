<template>
  <div class="line-container">
    <ResourceLine
      v-for="resourceEvents in this.sortedByResource"
      v-bind:key="resourceEvents.resource"
      :startTime="startTime"
      :endTime="endTime"
      :events="resourceEvents.events"
    ></ResourceLine>
  </div>
</template>
<script>
import ResourceLine from "./ResourceLine.vue";
import Vue from "vue";
import _ from "lodash";
Object.defineProperty(Vue.prototype, "$_", { value: _ });
export default {
  name: "LineContainer",
  components: {
    ResourceLine
  },
  props: {
    //array with event viz. notes.md
    events: Array,
    startTime: String,
    endTime: String
  },

  computed: {
    sortedByResource() {
      var grouped = this.groupBy();
      return grouped;
    }
  },
  methods: {
    groupBy() {
      var result = this.$_.chain(this.events)
        // Group the elements of Array based on `color` property
        .groupBy("resource")
        // `key` is group's name (color), `value` is the array of objects
        .map((value, key) => ({ resource: key, events: value }))
        .value();

      return result;
    }
  },
  created() {},
  mounted() {
    console.log(this.sortedByResource);
  }
};
</script>

<style scoped>
.line-container {
  margin: 5px;
  height: 20%;
  min-width: 40px;
  display: flex;
  flex-wrap: nowrap;
  flex-direction: row;
}
</style>