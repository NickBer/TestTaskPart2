<template>
  <ul class="ml-2">
    <li>
      Genres:
      <span v-for="(genre,index) in movie.genre_id" :key="index">{{genres[genre]}} </span>
    </li>
    <li>Date of release: {{dateOfRelease}}</li>
  </ul>
</template>

<script>
import genres from "../genres.js";
export default {
  props: {
    movie: Object
  },
  computed: {
    dateOfRelease() {
      let d = new Date(this.movie.dateOfRelease);
      let date = [
        "0" + d.getDate(),
        "0" + (d.getMonth() + 1),
      ].map(component => component.slice(-2));

      return `${date.join(".")}.${d.getFullYear()}`;
    }
  },
  data() {
    return {
      genres
    };
  }
};
</script>