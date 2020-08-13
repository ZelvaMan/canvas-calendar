<template>
  <div class="resource-line">
    <div
      class="line red"
      v-bind:style="createStyleString(div)"
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

      var startHour = moment(this.startTime, "hh:mm").format("HH");
      var endHour = moment(this.endTime, "hh:mm").format("HH");
      var hours = 24 - startHour - (24 - endHour);
      //percent per hour
      var pPHour = 100 / hours;
      //percent per quorter
      var pPQuarter = pPHour / 4;
      console.log(pPQuarter);
      var id = 0;
      var divs = [{ color: this.tcolor, height: "100", id: ++id }];
      var lastEventEnd = startHour;
      if (this.events != undefined) {
        divs = [];

        this.events.forEach(e => {
          var eStart = {
            hour: moment(e.start, "hh:mm").format("HH"),
            minute: moment(e.start, "hh:mm").format("mm")
          };
          var eEnd = {
            hour: moment(e.end, "hh:mm").format("HH"),
            minute: moment(e.end, "hh:mm").format("mm")
          };
          //fill blank space between last event end and this event start
          var blankDiv = {
            color: this.tcolor,
            height: (eStart.hour - lastEventEnd) * pPHour,
            id: ++id
          };
          divs.push(blankDiv);
          // add line
          console.log(eStart.hour);
          console.log(eEnd.hour);

          var divHeigh =
            (eEnd.hour - eStart.hour) * pPHour -
            this.minToQuarter(eStart.minute) * pPQuarter +
            this.minToQuarter(eEnd.minute) * pPQuarter;
          var eDiv = { color: e.color, height: Math.round(divHeigh), id: ++id };
          divs.push(eDiv);
          console.log("eDiv");
          console.log(divHeigh);
        });
      }

      console.log(this.divs);
      return divs;
    }
  },
  methods: {
    minToQuarter(minute) {
      return Math.round(minute / 15);
    },
    createStyleString(div) {
      var v = "height:" + div.height + "%;background:" + div.color + ";";
      return v;
    }
  }
};
</script>

<style scoped>
.resource-line {
  margin-left: 5px;
  width: 5px;
  height: 100%;
  display: flex;
  flex-wrap: nowrap;
  flex-direction: column;
}

.line {
  width: 100%;
}
.red {
  background: red;
}
</style>