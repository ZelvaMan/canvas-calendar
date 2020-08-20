<template>
  <div class="event-creator-line-container" :style="colorBorderStyleString">
    <div :style="colorStyleString" class="name-container">
      {{resourceInfo.name}}
      <div style="font-size: 0.6rem">({{resourceInfo.possision}})</div>
    </div>
    <div class="input-container">
      <textarea
        v-for="n in parseInt(daysOfWeek, 10)"
        :key="n"
        :ref="n"
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
import _ from "lodash";
import Vue from "vue";
//import moment from "moment";
Object.defineProperty(Vue.prototype, "$_", { value: _ });
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
    events: Array,
    daysOfWeek: String,
    weekDateStart: String,
    NameSize: Number
  },
  watch: {
    events() {
      //rerender all
      this.eventsByDay = this.eventsData(this.events);
      this.setTextAreasByEvents();
    }
  },
  methods: {
    setTextAreasByEvents() {
      console.log("setting text arreas");
      this.eventsGroupedByDay.forEach(d => {
        var diff = this.daysDiff(this.weekDateStart, d.date);
        var textarea = this.$refs[diff + 1][0];
        var str = this.createDayString(d.events);
        textarea.value = str;
      });
    },
    createDayString(events) {
      var str = "";
      events.forEach(e => {
        var s = this.createEventString(e);
        str += s;
        str += "\n";
      });
      return str;
    },
    createEventString(event) {
      var start = moment(event.start, "YYYY-MM-DD HH:mm").format("HH:mm");
      var end = moment(event.end, "HH:mm").format("HH:mm");
      var str = start + "-" + end;
      return str;
    },
    onChange(event) {
      this.onChangeHandler(event.target);
    },
    onChangeHandler(target) {
      console.log("Change trigered s:" + target.value);
      if (target.value.replace(/' '|\n/g, "").lenght < 3) {
        return;
      }
      var spl = target.value.split("\n");
      if (spl == undefined) spl = target.value;
      var id = target.id[0];
      var events = [];
      var sIfError = "";
      var error = false;
      spl.forEach((s, i) => {
        var e = this.createEvent(spl[i], id);
        console.log(target.value);
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
      this.eventsByDay[id - 1].events = events;
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
      this.$emit("input", {
        resource: this.resourceInfo.id,
        events: this.eventsToEmit()
      });
    },
    eventsToEmit() {
      var result = [];
      console.log("eventsbyday");
      console.log(this.eventsByDay);
      this.eventsByDay.forEach(events => {
        result = result.concat(events.events);
      });
      return result;
    },
    daysDiff(date1, date2) {
      var start = moment(date1, "YYYY-MM-DD");
      var end = moment(date2, "MM-DD");
      return Math.abs(moment.duration(start.diff(end)).asDays());
    },
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
    },
    eventsData(events) {
      var res = [];
      this.eventsGroupedByDay.forEach((d, i) => {
        res[i] = d;
      });
      return res;
    }
  },
  computed: {
    colorStyleString() {
      return (
        "color:" +
        this.resourceInfo.color +
        ";width:" +
        this.NameSize.toString() +
        "rem;"
      );
    },
    colorBorderStyleString() {
      return " border-bottom: 2px solid " + this.resourceInfo.color + ";";
    },
    inputDynamicStyle() {
      var width = 100 / this.daysOfWeek;
      return " width:" + width + "%;";
    },
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
        .map((value, key) => ({
          date: key,
          events: value
        }))
        .value();

      var cdate = moment(this.weekDateStart).format("MM.DD.");
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
  mounted() {
    this.setTextAreasByEvents();
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