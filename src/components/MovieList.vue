<template>
  <ul class="list-group">
    <li
      v-for="movie in movies"
      :class="{'active':selectedId===movie._id}"
      :key="movie._id"
      @click="$emit('select',selectedId=movie._id);"
      class="list-group-item list-group-item-action"
    >
      {{movie.name.toUpperCase()}}
      <br />
      {{movie.genre_id.map(id=>genres[id]).join(' ')}}
      <br>
      <small >Shows at {{startTime(getSession(movie._id))}}</small>
    </li>
  </ul>
</template>

<script>
import genres from "../genres.js";
export default {
  props: {
    movies: {
      type: Array,
      default() {
        return [{ _id: 0 }];
      }
    },
    sessions:Array
  },
  computed:{
    
  },
  methods: {
    startTime(session) {
      let time = new Date();
      try {
        time = new Date(session.startTime);
      } catch (error){

        // eslint-disable-next-line no-console
        console.log(error)
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
    },
    getSession(movie_id){
      return this.sessions.filter(
        session => session.movie_id === movie_id
      )[0];
    }
  },
  data() {
    return {
      genres,
      selectedId: this.movies[0]._id
    };
  }
};
</script>

<style scoped>
  ul{
    overflow: scroll;
    max-height: 70vh;
  }
</style>