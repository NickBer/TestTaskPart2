<template>
  <div class="ml-5">
    <h2>Session:</h2>
    <p>Start time: {{startTime}}</p>
    <p>Ticket price: {{filteredSession.ticketPrice}} UAH</p>
    <movie-session-book :session="filteredSession"></movie-session-book>
  </div>
</template>

<script>
import MovieSessionBook from "./MovieSessionBook.vue";
export default {
  components: { MovieSessionBook },
  props: {
    sessions: Array,
    id: String
  },
  computed: {
    filteredSession() {
      let filtered = this.sessions.filter(
        session => session.movie_id === this.id
      );
      return filtered[0];
    },
    startTime() {
      let time = new Date();
      try {
        time = new Date(this.filteredSession.startTime);
      } catch (error) {
        // eslint-disable-next-line no-console
        console.log(error);
      }
      let d = time;
      d = [
        "0" + d.getDate(),
        "0" + (d.getMonth() + 1),
        "" + d.getFullYear(),
        "0" + d.getHours(),
        "0" + d.getMinutes()
      ].map(component => component.slice(-2));
      return d.slice(0, 3).join(".") + " " + d.slice(3).join(":");
    }
  }
};
</script>