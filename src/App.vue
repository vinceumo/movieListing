<template>
  <div id="movieListingApp">
    <AppHeader title="Films Showing in Cinemas"/>
    <MovieListing v-if="movies.length" v-bind:movieList="movies" />
  </div>
</template>

<script>
import AppHeader from "./components/organisms/AppHeader";
import MovieListing from "./components/organisms/MovieListing";
import axios from "axios";

export default {
  name: "app",
  components: {
    AppHeader,
    MovieListing
  },
  data() {
    return {
      movies: [],
      movieGenres: {},
      country: "GB"
    };
  },
  created: function() {
    this.getMovieGenres();
    this.getPopularMovies();
  },
  methods: {
    sortObjectBy(property) {
      return function(a, b) {
        return a[property] == b[property]
          ? 0
          : +(a[property] > b[property]) || -1;
      };
    },
    getPopularMovies() {
      const _this = this;
      axios
        .get(
          `https://api.themoviedb.org/3/movie/now_playing?api_key=386f3833fc837a1939fab52920de1c25&page=1&region=${
            _this.country
          }`
        )
        .then(function(response) {
          let results = response.data.results;
          results = results.sort(_this.sortObjectBy("popularity")); // Output films from low to high popularity
          for (let result of results) {
            let genreArr = [];

            for (let genreId of result.genre_ids) {
              genreId = _this.movieGenres[genreId];
              genreArr.push(genreId);
            }

            result.genres = genreArr;
          }
          _this.movies = results.reverse();
        })
        .catch(function(error) {
          //Todo error fallback component
          console.log(error);
        });
    },
    getMovieGenres() {
      const _this = this;
      axios
        .get(
          "https://api.themoviedb.org/3/genre/movie/list?api_key=386f3833fc837a1939fab52920de1c25&language=en-GB"
        )
        .then(function(response) {
          let genresArr = response.data.genres;
          for (let genre of genresArr) {
            _this.movieGenres[genre.id] = genre.name;
          }
        })
        .catch(function(error) {
          //Todo error
          console.log(error);
        });
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Lato:400,400i,700|Righteous&display=swap");

body {
  background-color: color(secondary, dark);
}
</style>
