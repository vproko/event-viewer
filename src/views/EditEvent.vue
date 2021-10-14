<template>
  <h1>{{ title }}</h1>
  <form @submit.prevent="editEvent">
    <input type="hidden" name="id" v-model="getSelectedEvent.id" />
    <div>
      <label>NAME</label>
      <input type="text" name="name" v-model="getSelectedEvent.name" />
    </div>
    <div>
      <label>DATE</label>
      <input type="date" name="date" v-model="getSelectedEvent.date" />
    </div>
    <div>
      <label>DESCRIPTION</label>
      <textarea
        type="text"
        name="description"
        v-model="getSelectedEvent.description"
      ></textarea>
    </div>
    <div>
      <button class="submit">UPDATE EVENT</button>
    </div>
  </form>
  <div v-if="this.getMessage">
    <p class="message">{{ this.getMessage }}</p>
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
export default {
  name: "EditEvent",
  data() {
    return {
      title: "Edit Event",
      eventsCopy: {}
    };
  },
  async created() {
    await this.findSelectedEvent(this.$route.params.id);
  },
  beforeUnmount() {
    this.clearTheMessage();
  },
  methods: {
    ...mapActions([
      "updateEvent",
      "findSelectedEvent",
      "setTheMessage",
      "clearTheMessage",
    ]),
    editEvent(e) {
      let updatedEvent = {};
      for (const key in this.getSelectedEvent) {
        if (!e.target[key].value) {
          return this.setTheMessage("There's empty field/s");
        }
        updatedEvent[key] = key !== "id" ? e.target[key].value : Number(e.target[key].value);
      }
      this.updateEvent(updatedEvent);
      this.setTheMessage("The event was successfully edited");
    },
  },
  computed: {
    ...mapGetters(["getSelectedEvent", "getMessage"]),
  },
};
</script>

<style scoped></style>
