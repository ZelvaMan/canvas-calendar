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
        v-on:change="onChange"
        v-on:keyup.space="onSpace"
        oninput="this.style.height = '';this.style.height = this.scrollHeight + 'px'"
      />
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "EventCreatorLine",
  data() {
    return {
      eventsByDay: []
    };
  },
  props: {
    //  must contain name color id and default possision
    resourceInfo: Object,
    daysOfWeek: String,
    weekDateStart: String
  },
  methods: {
    onChange(event) {
      this.onChangeHandler(event.target);
    },
    onChangeHandler(target) {
      console.log("Change trigered s:" + target.value);
      if (target.value.replace(/' '|\n/g, "").lenght < 3) {
        return;
      }
      var spl = target.value.split("\n");
      var id = target.id[0];
      var events = [];
      var sIfError = "";
      var error = false;
      spl.forEach((s, i) => {
        var e = this.createEvent(spl[i], id);
        console.log("e");
        console.log(e);
        //if error in crearing event ( propablly parsing)
        if (e == null) {
          error = true;
        } else {
          events.push(e);

          sIfError += s + "\n";
        }
      });
      console.log(events);
      if (error == true) {
        console.log("error ");
        target.value = sIfError;
      }
      //
      this.eventsByDay[id] = events;
      this.emit();
    },

    createEvent(string, id) {
      var date = moment(this.weekDateStart, "YYYY-MM-DD").add(id - 1, "d");
      var times = this.parseTimes(string);
      var result = null;
      if (!(times.start > times.end)) {
        var eventObject = {
          start: date.format("YYYY-MM-DD") + " " + times.start,
          end: times.end,
          color: this.resourceInfo.color,
          resource: this.resourceInfo.id,
          possision: this.resourceInfo.possision
        };
        result = eventObject;
      } else {
        result = null;
      }
      if (string == " " || string == "") {
        result = null;
        ("ERROR");
      }
      console.log("result");
      console.log(result);
      return result;
    },
    parseTimes(string) {
      console.log(string);
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
      this.onChangeHandler(e.target);
    },
    emit() {
      console.log("emitting ");
      console.log(this.eventsToEmit());
      this.$emit("input", this.eventsToEmit());
    },
    eventsToEmit() {
      var result = [];
      this.eventsByDay.forEach(events => {
        result = result.concat(events);
      });
      return result;
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