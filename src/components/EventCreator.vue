<template>
  <div class="container">
    <div class="dates-container">
      <div :style="'width:' + (getNameWidth/1.7).toString()  + 'rem;'" class="white-space">
        <button v-on:click="reset">
          <i class="fa fa-undo" aria-hidden="true"></i>
        </button>
      </div>
      <div class="date-container" v-for="n in parseInt(daysOfWeek)" :key="n">
        {{moment(startDate).add(n -1,"d").format("MM.DD.")}}
        <br />
        <div style="font-size:0.8rem;">( {{moment(startDate).add(n -1,"d").format("dddd")}})</div>
      </div>
      <div class="date-container">
        <b>
          celkem hodin
          <br />za týden
        </b>
      </div>
      <div class="date-container">
        <b>
          celkem hodin
          <br />za měsíc
        </b>
      </div>
    </div>
    <EventCreatorLine
      v-for="ri in selectedResourceInfos"
      :key="ri.id"
      :events="getEventWithResource(ri.id)"
      :resourceInfo="ri"
      :daysOfWeek="daysOfWeek"
      :weekDateStart="startDate"
      :possisions="possisions"
      :NameSize="getNameWidth"
      v-on:input="onInput"
      :startTime="startTime"
      :endTime="endTime"
      :totalHoursMonth="getTotalHoursMonth(ri.id)"
    ></EventCreatorLine>
    <div class="select">
      <multiselect
        v-model="selectSelected"
        :options="unselectedRINames"
        v-on:input="SelectInputHandler"
      ></multiselect>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
import Vue from "vue";

import moment from "moment";
//import moment from "moment";
Object.defineProperty(Vue.prototype, "$_", { value: _ });
import EventCreatorLine from "./EventCreatorLine";
export default {
  name: "EventCreator",
  components: {
    EventCreatorLine,
  },
  data() {
    return {
      selectSelected: null,

      eventsByResource: this.groupByResource(),
      unselectedResourceInfos: this.getDefaultHidden(),
      selectedResourceInfos: this.getDefaultDisplayed(),
    };
  },
  props: {
    daysOfWeek: String,
    startDate: String,
    events: Array,
    resourceInfos: Array,
    possisions: Array,

    startTime: String,
    endTime: String,
  },
  watch: {
    events() {
      this.eventsByResource = this.groupByResource();
    },
  },
  mounted() {},
  methods: {
    onInput(e) {
      //need to fix emit so it will join with other resources
      this.$emit("input", this.eventsForEmit(e));
    },

    getResourceInfo(resource) {
      var res = null;
      this.resourceInfos.forEach((rs) => {
        if (rs.id == resource) res = rs;
      });
      return res;
    },

    getEventWithResource(resource) {
      var result = [];
      var events = [];
      var inWeek = moment(this.startDate)
        .add(this.daysOfWeek, "days")
        .format("YYYY/MM/DD");
      this.eventsByResource.forEach((e) => {
        if (e.resource == resource) {
          events = e.events;
        }
      });
      events.forEach((e) => {
        if (moment(e.start).isBetween(this.startDate, inWeek)) {
          result.push(e);
        }
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
      console.log(groupedResource);
      this.eventsByResource.forEach((r) => {
        console.log(r);
        if (r.resource == groupedResource.resource) {
          console.log("Emminted possision");
          //* is resource curently emited
          var eventsForResource = [];
          concated = true;
          var inWeek = moment(groupedResource.startDate)
            .add(groupedResource.daysOfWeek, "days")
            .format("YYYY/MM/DD");
          var sd = moment(groupedResource.startDate)
            .add(-1, "days")
            .format("YYYY/MM/DD");
          console.log(inWeek);
          //* foreach all event with that resource
          r.events.forEach((e) => {
            //! events isnt from that week
            if (!moment(e.start, "YYYY/MM/DD").isBetween(sd, inWeek)) {
              //! add event to events for resourcec
              eventsForResource.push(e);
              console.log("isnt between ");
              console.log(e);
            }
          });
          //
          console.log("added events that arent from that week");
          console.log(eventsForResource);
          //ad events from emit
          eventsForResource = eventsForResource.concat(groupedResource.events);
          events = events.concat(eventsForResource);
        } else {
          events = events.concat(r.events);
        }
      });
      if (!concated) {
        events = events.concat(groupedResource.events);
      }
      return events;
    },
    getTotalHoursMonth(resourceId) {
      var total = 0;
      var events = [];
      var month = moment(this.startDate, "YYYY/MM/DD").format("MM");
      this.events.forEach((e) => {
        var isSameMonth =
          moment(e.start, "YYYY/MM/DD HH:mm").format("MM") == month;
        if (e.resource == resourceId && isSameMonth) {
          events.push(e);
        }
      });
      events.forEach((e) => {
        var start = moment(
          moment(e.start, "YYYY/MM/DD HH:mm").format("HH:mm"),
          "HH:mm"
        );
        var end = "";
        if (e.end == "cl") {
          end = moment(this.endTime, "HH:mm");
        } else {
          end = moment(e.end, "HH:mm");
        }

        var range = moment.duration(start.diff(end));
        var hours = range.asHours();
        total += Math.abs(hours);
      });
      console.log("total for month for " + resourceId);

      console.log(total);
      return total.toString();
    },
    SelectInputHandler() {
      console.log("select input name:" + this.selectSelected);
      if (this.selectSelected == null) {
        return;
      }
      //add resource to visible resources
      var ri = this.getResourcecByName(
        this.unselectedResourceInfos,
        this.selectSelected
      );
      this.selectedResourceInfos.push(ri);
      //delete from unselected resources
      this.unselectedResourceInfos.splice(
        this.unselectedResourceInfos.findIndex(
          (v) => v.name === this.selectSelected
        ),
        1
      );
      //deselct
      this.selectSelected = null;
    },
    getResourcecByName(resourceInfos, name) {
      var result;
      resourceInfos.forEach((ri) => {
        if (ri.name == name) {
          result = ri;
          return result;
        }
      });
      return result;
    },
    getDefaultDisplayed() {
      var displayed = [];
      this.resourceInfos.forEach((ri) => {
        if (!ri.hidden || ri.hidden == undefined) {
          displayed.push(ri);
        }
      });
      return displayed;
    },
    getDefaultHidden() {
      var hidden = [];
      this.resourceInfos.forEach((ri) => {
        if (ri.hidden) {
          hidden.push(ri);
        }
      });
      return hidden;
    },
    reset() {
      this.unselectedResourceInfos = this.getDefaultHidden();
      this.selectedResourceInfos = this.getDefaultDisplayed();
    },
  },
  computed: {
    getNameWidth() {
      var longest = this.resourceInfos.reduce(function (a, b) {
        return a.name.length > b.name.length ? a : b;
      });
      return longest.name.length;
    },
    unselectedRINames() {
      var names = [];
      this.unselectedResourceInfos.forEach((ri) => {
        names.push(ri.name);
      });
      console.log(names);
      return names;
    },
    //get names of undisplayed resources
  },
};
</script>
<style scoped>
.container {
  margin-top: 25px;
}
.dates-container {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  width: auto;
  border-bottom: 2px solid black;
}

.date-container {
  margin: 1px;
  width: 6rem;
}
.white-space {
  margin: 1px;
  border-left: solid transparent 5px;
}
.select {
  padding: 1px;
  width: 30%;
}
</style>  