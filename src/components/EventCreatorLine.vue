<template>
  <div class="event-creator-line-container" :style="colorBorderStyleString">
    <div :style="colorStyleString">
      {{resourceInfo.name}}
      <div style="font-size: 0.6rem">({{resourceInfo.possision}})</div>
    </div>
    <div class="input-container">
      <textarea
        v-for="n in parseInt(daysOfWeek, 10)"
        :key="n"
        :id="   n+ '-' +resourceInfo.id "
        type="text"
        :style="inputDynamicStyle"
        v-on:change="onInput"
        v-on:keyup.space="onSpace"
      />
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "EventCreatorLine",
  props: {
    //  must contain name color id and default possision
    resourceInfo: Object,
    daysOfWeek: String,
    weekDateStart: String
  },
  methods: {
    onInput(event) {
      var date = moment(this.weekDateStart, "YYYY-MM-DD").add(
        event.target.id[0] - 1,
        "d"
      );
      var times = this.parseTimes(event.target.value);

      if (times.start > times.end) {
        event.target.value = "";
      } else {
        var eventObject = {
          start: date.format("YYYY-MM-DD") + " " + times.start,
          end: times.end,
          color: this.resourceInfo.color,
          resource: this.resourceInfo.id,
          possision: this.resourceInfo.possision
        };
        this.$emit("input", eventObject);
      }
    },
    parseTimes(string) {
      var result = { start: "", end: "" };
      var splitted = string.split("-");
      //start
      result.start = moment(splitted[0], "HH:mm").format("HH:mm");

      //end
      result.end = moment(splitted[1], "HH:mm").format("HH:mm");
      return result;
    },
    onSpace(e) {
      e.preventDefault();
      e.target.value += "\n";
      e.target.sty = e.target.rows * (1.1 * e.target.fontSize);
      console.log(e.target.clientHeight);
    }
  },
  computed: {
    colorStyleString() {
      return "color:" + this.resourceInfo.color + ";";
    },
    colorBorderStyleString() {
      return " border-bottom: 2px solid " + this.resourceInfo.color + ";";
    },
    inputDynamicStyle() {
      var width = 100 / this.daysOfWeek;
      return " width:" + width + "%;";
    }
  }
};
</script>

<style scoped>
.event-creator-line-container {
  display: flex; /* or inline-flex */
  flex-direction: row;
  flex-wrap: nowrap;
  margin: 10px;
  width: auto;
}
.input-container {
  display: flex;
  height: auto;
}
textarea {
  margin: 5px;
  width: auto;
  resize: both;
  overflow: none;
}
</style>