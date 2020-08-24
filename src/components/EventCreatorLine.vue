<template>
  <div class="event-creator-line-container" :style="this.colorBorderStyleString">
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
    // !  must contain name color id and default possision
    resourceInfo: Object,

    startTime: String,
    endTime: String,
    events: Array,
    daysOfWeek: String,
    weekDateStart: String,
    NameSize: Number,
    possisions: Array
  },
  watch: {
    events() {
      //* render events from this.events
      this.eventsByDay = this.eventsData(this.events);
      this.setTextAreasByEvents();
    }
  },
  methods: {
    // sets default texts for textaras
    setTextAreasByEvents() {
      this.eventsGroupedByDay.forEach(d => {
        //difference between start date an date, used to calculate textare ID
        var diff = this.daysDiff(this.weekDateStart, d.date);
        //get textarea ref based on days between date and weekDateStart
        var textarea = this.$refs[diff + 1][0];
        //get event string
        var str = this.createDayString(d.events);
        //set textare to that string
        textarea.value = str;
      });
    },
    //create day string for events to be render in textareas
    createDayString(events) {
      //create variables
      var str = "";

      //foreach events and create string for each one and joint them together
      events.forEach(e => {
        var s = this.createEventString(e);
        str += s;
        str += "\n";
      });
      return str;
    },
    //create string for  event
    createEventString(event) {
      //get start and end in right formats
      var start = moment(event.start, "YYYY/MM/DD HH:mm").format("HH:mm");
      var end = moment(event.end, "HH:mm").format("HH:mm");
      var possision = "";

      //check if event have set different possision
      if (event.possision != this.resourceInfo.possision) {
        //in that case set first char of string to first char of possision
        possision = event.possision.charAt(0);
      }
      //joint string together
      var str = possision + start + "-" + end;
      return str;
    },

    //event handlerer for text areasa
    onChange(event) {
      this.onChangeHandler(event.target);
    },
    //methof fot hangeling onchange events
    onChangeHandler(target) {
      console.log("Change trigered s:" + target.value);

      //if value is too small to be valid return
      if (target.value.replace(/' '|\n/g, "").lenght < 3) {
        return;
      }
      //split string by \n
      var spl = target.value.split("\n");
      //*if there is only one event
      if (spl == undefined) spl = target.value;

      //get target id
      var id = target.id[0];

      //set default values
      var events = [];
      //* valid strings
      var sIfError = "";
      var error = false;

      //foreach strings and create events for every one of them
      spl.forEach((s, i) => {
        var e = this.createEvent(spl[i], id);
        //if error in crearing event ( propablly parsing)
        if (e == null) {
          error = true;
        } else {
          //add to events and to valid string
          events.push(e);
          sIfError += s + "\n";
        }
      });
      if (error == true) {
        //set value to have only valid events
        target.value = sIfError;
      }
      //set events
      this.eventsByDay[id - 1].events = events;
      //emit
      this.emit();
    },
    //create event
    createEvent(string, id) {
      var date = moment(this.weekDateStart, "YYYY/MM/DD").add(id - 1, "d");
      var c = string.charAt(0);
      var possision = this.resourceInfo.possision;
      if (!(c >= "0" && c <= "9")) {
        //* if first character isnt number
        possision = this.getPossision(c);
        //delete first car
        string = string.substring(1);
      }
      var times = this.parseTimes(string);
      var result = null;
      if (!(times.start > times.end)) {
        var eventObject = {
          start: date.format("YYYY/MM/DD") + " " + times.start,
          end: times.end,
          color: this.resourceInfo.color,
          resource: this.resourceInfo.id,
          possision: possision
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
      if (splitted[1] == "cl") {
        result.end = this.endTime;
      } else {
        result.end = moment(splitted[1], "HH:mm").format("HH:mm");
      }
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
      console.log("date2 string");
      console.log(date2);
      var start = moment(date1, "YYYY/MM/DD");
      var end = moment(date2, "MM/DD");
      console.log("start");
      console.log(start);
      console.log("end");
      console.log(end);
      console.log(date2);
      return Math.abs(moment.duration(start.diff(end)).asDays());
    },
    haveDate(array, date) {
      var tdate = moment(date, "MM/DD").format("MM/DD");
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
    },
    getPossision(char) {
      // TODO GET POSSISIONS TO COMPONENT
      // var res = "";

      // this.possisions.forEach(r => {
      //   if (char == r.atChar(0)) res = r;
      // });
      // return res;
      char = char.toLowerCase();
      if (char == "p") return "Pizza";
      if (char == "u") return "Uklid";
      if (char == "b") return "Bar";
      if (char == "s") return "Servis";
      if (char == "k") return "Kuchar";
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
        console.log("date form eventsWDate");
        console.log(moment(e.start, "YYYY/MM/DD HH:mm").format("MM/DD"));
        e.date = moment(e.start, "YYYY/MM/DD HH:mm").format("MM/DD");
        console.log(e.date);
      });
      console.log(events);
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

      var cdate = moment(this.weekDateStart, "YYYY/MM/DD").format("MM/DD");
      for (let i = 0; i < 5; i++) {
        var d = this.haveDate(byDate, cdate);
        if (d != undefined) {
          result.push(d);
        } else {
          result.push({ date: cdate, events: [] });
        }
        cdate = moment(cdate, "MM/DD")
          .add(1, "d")
          .format("MM/DD");
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
  width: 100%;
}
textarea {
  margin: 5px;
  width: auto;
  resize: both;
  overflow: none;
}
</style>