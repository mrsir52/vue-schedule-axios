<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="9">
        <h2 class="text-uppercase my-3">Today:</h2>
        <Event
          v-for="(event, index) in events"
          :index="index"
          :time="event.time"
          :title="event.title"
          :location="event.location"
          :description="event.description"
          :key="index"
          @delete="handleDelete"
        />
        <mdb-row>
          <mdb-col xl="3" md="6" class="mx-auto text-center">
            <mdb-btn color="info" @click.native="modal = true">Add Event</mdb-btn>
          </mdb-col>
        </mdb-row>
      </mdb-col>
      <mdb-col col="3">
        <h3 class="text-uppercase my-3">Schedule</h3>
        <h5 class="my-3">
           You have
          <b>{{events.length}} events</b> today.
        </h5>
        <div class="custom-control custom-checkbox">
          <input type="checkbox" class="custom-control-input" id="show" v-model="showMore"/>
          <label class="custom-control-label" for="show">Show temp in Honolulu</label>
        </div>
        <h1 class="my-3">
          <mdb-row>
            <mdb-col col="3" class="text-center" v-show="showMore">
              <mdb-icon far icon="sun" />
            </mdb-col>
            <mdb-col col="9" v-show="showMore">Sunny</mdb-col>
          </mdb-row>
          <mdb-row>
            <mdb-col col="3" class="text-center" v-show="showMore">
              <mdb-icon icon="thermometer-three-quarters" />
            </mdb-col>
            <mdb-col col="9" v-show="showMore">{{ info.data.main.temp }}°F</mdb-col>
          </mdb-row>
        </h1>
      </mdb-col>
    </mdb-row>

    <mdb-modal v-if="modal" @close="modal = false">
      <mdb-modal-body>
        <form class="mx-3 grey-text">
          <mdb-input
            name="time"
            label="Time"
            icon="clock"
            placeholder="12:30"
            type="time"
            @input="handleInput($event, 'time')"
          />
          <mdb-input
            name="title"
            label="Title"
            icon="edit"
            placeholder="Briefing"
            type="text"
            @input="handleInput($event, 'title')"
          />
          <mdb-input
            name="location"
            label="Location (optional)"
            icon="map"
            type="text"
            @input="handleInput($event, 'location')"
          />
          <mdb-textarea
            name="description"
            label="Description (optional)"
            icon="sticky-note"
            @input="handleInput($event, 'description')"
          />
        </form>
        <mdb-btn color="info" @click.native="addEvent">Add</mdb-btn>
      </mdb-modal-body>
    </mdb-modal>
    <!-- <mdb-btn color="info" @click.native="loadWeather">Load Weather</mdb-btn> -->
  </mdb-container>
</template>

<script>
import {
  mdbContainer,
  mdbRow,
  mdbCol,
  mdbIcon,
  mdbBtn,
  mdbModal,
  mdbModalHeader,
  mdbModalTitle,
  mdbModalBody,
  mdbModalFooter,
  mdbInput,
  mdbTextarea
} from "mdbvue";
import * as axios from "axios";
import Event from "@/components/Event";
export default {
  name: "App",
  components: {
    mdbContainer,
    mdbRow,
    mdbCol,
    mdbIcon,
    mdbBtn,
    mdbModal,
    mdbModalHeader,
    mdbModalTitle,
    mdbModalBody,
    mdbModalFooter,
    mdbInput,
    mdbTextarea,
    Event
  },
  data() {
    return {
      events: [
        {
          time: "10:00",
          title: "Breakfast with Simon",
          location: "Lounge Caffe",
          description: "Discuss Q3 targets"
        },
        {
          time: "10:30",
          title: "Daily Standup Meeting (recurring)",
          location: "Warsaw Spire Office"
        },
        {
          time: "11:00",
          title: "Call with HRs"
        },
        {
          time: "12:00",
          title: "Lunch with Timmoty",
          location: "Canteen",
          description: "Project evalutation "
        }
      ],
      info: null,
      modal: false,
      newValues: [],
      showMore: false
    };
  },
  mounted() {
    axios
      .get(
        "https://api.openweathermap.org/data/2.5/weather?q=Honolulu&appid=639a38a8db7744d2e5a331c5b715a92e&units=imperial"
      )
      .then(response => (this.info = response));
  },
  methods: {
    handleDelete(eventIndex) {
      this.events.splice(eventIndex, 1);
    },
    handleInput(val, type) {
      this.newValues[type] = val;
    },
    addEvent() {
      this.events.push({
        time: this.newValues["time"],
        title: this.newValues["title"],
        location: this.newValues["location"],
        description: this.newValues["description"]
      });
    }
  }
};
</script>

<style>
</style>

