<template>
  <div class="container">
    <EventCreatorLine
      :resourceInfo="resourceInfo"
      :daysOfWeek="daysOfWeek"
      weekDateStart="2020-08-10"
      v-on:input="onInput"
    ></EventCreatorLine>
    <EventCreatorLine
      :resourceInfo="resourceInfo2"
      :daysOfWeek="daysOfWeek"
      weekDateStart="2020-08-10"
      v-on:input="onInput"
    ></EventCreatorLine>

    <EventCreatorLine
      :resourceInfo="resourceInfo3"
      :daysOfWeek="daysOfWeek"
      weekDateStart="2020-08-10"
      v-on:input="onInput"
    ></EventCreatorLine>
  </div>
</template>

<script>
import EventCreatorLine from "./EventCreatorLine";
export default {
  name: "EventCreator",
  components: {
    EventCreatorLine
  },
  data() {
    return {
      resourceInfo: {
        color: "blue",
        name: "Ozias",
        id: "Ozias",
        possision: "KP"
      },
      resourceInfo2: {
        color: "red",
        name: "Adam",
        id: "Adam",
        possision: "Pizza"
      },
      resourceInfo3: {
        color: "green",
        name: "Jakub",
        id: "Jakub",
        possision: "Uklid"
      }
    };
  },
  props: {
    daysOfWeek: String
  },
  mounted() {
    console.log(this.resourceInfo);
  },
  methods: {
    onInput(e) {
      this.$emit("input", e);
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