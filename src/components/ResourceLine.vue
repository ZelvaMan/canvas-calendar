<template>
  <div class="resource-line">
    <span></span>
    <div
      class="line"
      v-bind:height="div.height + '%'"
      v-for="div in this.divsComputed"
      v-bind:key="div.id"
    ></div>
  </div>
</template>

<script>
import moment from "moment";
import Vue from "vue";
Vue.prototype.moment = moment;
export default {
  name: "ResourceLine",
  data() {
    return {
      // height and color
      tcolor: "rgba(0, 0, 0, 0);"
    };
  },
  props: {
    startTime: String,
    endTime: String,
    //props: start end color
    events: {
      type: Array
    }
  },
  mounted() {
    console.log("mounted fired");
    console.log(this.startTime);
  },
  beforeUpdated() {
    console.log("bupdate fired");
  },

  computed: {
    divsComputed() {
      console.log("generating divs ...");
      console.log(this.events);

      var startHour = moment(this.startTime).format("h");
      var endHour = moment(this.endTime).format("h");
      var hours = 24 - (startHour + (24 - endHour));
      //percent per hour
      var pPHour = 100 / hours;
      //percent per quorter
      var pPQuarter = pPHour / 4;
      var id = 0;
      var divs = [{ color: this.tcolor, height: "100", id: ++id }];
      var lastEventEnd = startHour;
      if (this.events != undefined) {
        divs = [];

        this.events.forEach(e => {
          var eStart = {
            hour: moment(e.start).format("h"),
            minute: moment(e.start).format("mm")
          };
          var eEnd = {
            hour: moment(e.end).format("h"),
            minute: moment(e.end).format("mm")
          };
          //fill blank space between last event end and this event start
          var blankDiv = {
            color: this.tcolor,
            height: (eStart.hour - lastEventEnd) * pPHour,
            id: ++id
          };
          divs.push(blankDiv);
          // add line
          var divHeigh =
            (eEnd.hour - eStart.hour) * pPHour -
            this.minToQuarter(eStart.minute) * pPQuarter +
            this.minToQuarter(eEnd.minute) * pPQuarter;

          var eDiv = { color: e.color, height: divHeigh, id: ++id };
          divs.push(eDiv);
        });
      }

      console.log(this.divs);
      return divs;
    }
  },
  methods: {
    minToQuarter(minute) {
      return Math.ceil(minute / 10);
    }
  }
};
</script>

<style scoped>
.resource-line {
  margin-left: 5px;
  width: 5px;
  display: flex;
  flex-wrap: nowrap;
  flex-direction: column;
}

.line {
  width: 100%;
}
</style>