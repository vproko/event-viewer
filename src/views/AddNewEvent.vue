<template>
  <h1>{{ title }}</h1>
  <form @submit.prevent="createNewEvent">
    <div>
      <label>NAME</label>
      <input type="text" v-model="newEvent.name" />
    </div>
    <div>
      <label>DATE</label>
      <input type="date" v-model="newEvent.date" />
    </div>
    <div>
      <label>DESCRIPTION</label>
      <textarea type="text" v-model="newEvent.description"></textarea>
    </div>
    <div>
      <button class="submit">CREATE EVENT</button>
    </div>
  </form>
  <div v-if="this.getMessage">
    <p class="message">{{ this.getMessage }}</p>
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
export default {
  name: "AddNewEvent",
  data() {
    return {
      title: "Add New Event",
      newEvent: {
        name: null,
        date: null,
        description: null,
      },
    };
  },
  beforeUnmount() {
    this.clearTheMessage();
  },
  methods: {
    ...mapActions(["addNewEvent", "clearTheMessage", "setTheMessage"]),
    createNewEvent() {
      for (const key in this.newEvent) {
        if (this.newEvent[key] === "" || this.newEvent[key] === null) {
          return this.setTheMessage("All field/s must be filled");
        }
      }
      let event = { ...this.newEvent };
      event.id = Math.round(Math.random() * 1000);
      this.addNewEvent(event);
    },
  },
  computed: {
    ...mapGetters(["getMessage"]),
  },
};
</script>

<style scoped></style>
