<template>
  <div class="row">
    <div class="col-sm-5">
      <img class="rounded float-right" :src="movie.pictureLink" alt="Poster" />
    </div>
    <div class="col-sm-7">
      <h1>{{movie.name.toUpperCase()}}</h1>
      <p>{{movie.description}}</p>
      <movie-genre-and-release-date :movie="movie" :id='movie._id'></movie-genre-and-release-date>
    </div>
    <movie-sessions :sessions="sessions" :id="movie._id"></movie-sessions>
    </div>
    

</template>

<script>
import MovieGenreAndReleaseDate from './MovieGenreAndReleaseDate.vue'
import MovieSessions from './MovieSessions.vue'
export default {
  props: {
    movie: Object,
    sessions:Array
  },
  components:{
    MovieGenreAndReleaseDate,
    MovieSessions
  }
};
</script>

<style scoped>
  img{
    max-width: 100%;
    max-height: 100%;
  }
</style>