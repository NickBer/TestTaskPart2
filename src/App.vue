<template>
  <div id="app">
    <div class="container-fluid px-5 pt-5 ">
      <div class="row">
        <div class="col-md-3">
          <div class="form-group">
            <label for="search">Search:</label>
            <input
              v-model="searchTitle"
              type="text"
              class="form-control"
              id="search"
              placeholder="Name..."
            />
            <select v-model="searchGenres" class="form-control" multiple>
              <option v-for="(genre,ind) in genres" :key="ind" :value="ind">{{genre}}</option>
            </select>
          </div>
          <hr />
          <MovieList :movies="filteredMovies" :sessions="sessions" @select="selectMovie"></MovieList>
          <hr />
        </div>
        <div class="col-md-9">
          <movie-information :movie="movie" :sessions="sessions"></movie-information>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MovieList from "./components/MovieList.vue";
import MovieInformation from "./components/MovieInformation.vue";
import ky from "ky";
import genres from "./genres.js";

export default {
  name: "app",
  components: {
    MovieList,
    MovieInformation
  },
  data() {
    return {
      movies: [],
      movie: {
        pictureLink: "#",
        name: "",
        description: "",
        genre_id: []
      },
      searchTitle: "",
      searchGenres: [],
      genres,

      sessions: [{movie_id:''}]
    };
  },
  computed: {
    filteredMovies() {
      let filtered = this.movies.filter(movie =>
        movie.name.includes(this.searchTitle)
      );
      if (this.searchGenres.length) {
        filtered = filtered.filter(movie => {
          let fit = true;
          for (let genre of this.searchGenres) {
            if (!movie.genre_id.includes(genre)) fit = false;
          }
          return fit;
        });
      }
      if (!this.movies.length) return [this.movie];
      return filtered;
    }
  },
  methods: {
    selectMovie(id) {
      for (const movie of this.movies) {
        if (movie._id == id) {
          this.movie = movie;
          break;
        }
      }
    }
  },
  mounted() {
    ky.get("https://cinema-api-test.herokuapp.com/movies")
      .json()
      .then(res => {
        this.movies = res;
        this.movie = this.movies[0];
      });
    ky.get("https://cinema-api-test.herokuapp.com/movieShows")
      .json()
      .then(res => {
        this.sessions = res;
      });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
