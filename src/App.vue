<template>
  <div id="movieListingApp">
    <AppHeader title="Films Showing in Cinemas" />
    <main>
      <FilterBar v-on:ratingValChange="getRatingValue" />
      <LoadingSpinner v-if="!hasApiCallError && movies.length === 0" />
      <transition name="fade">
        <div v-if="hasApiCallError">
          <ErrorMessage
            copy="There was an error, please try again"
            gif="https://media.giphy.com/media/3ohzdYJK1wAdPWVk88/giphy.gif"
          />
        </div>
        <MovieListing
          v-if="movies.length"
          v-bind:movieList="movies"
          v-bind:selectedRating="parseInt(selectedRating)"
        />
      </transition>
    </main>
  </div>
</template>

<script>
import AppHeader from "./components/organisms/AppHeader";
import ErrorMessage from "./components/molecules/ErrorMessage";
import FilterBar from "./components/organisms/FilterBar";
import LoadingSpinner from "./components/molecules/LoadingSpinner";
import MovieListing from "./components/organisms/MovieListing";
import axios from "axios";
import cssVars from "css-vars-ponyfill";

export default {
  name: "app",
  components: {
    AppHeader,
    ErrorMessage,
    FilterBar,
    LoadingSpinner,
    MovieListing
  },
  data() {
    return {
      movies: [],
      movieGenres: {},
      country: "GB", // Todo implement country picker
      selectedRating: 0,
      hasApiCallError: false,
      apiCallErrorMsg: ""
    };
  },
  created: function() {
    this.getMovieGenresAndMovies();
  },
  mounted: function() {
    cssVars();
  },
  methods: {
    sortObjectBy(property) {
      return function(a, b) {
        return a[property] == b[property]
          ? 0
          : +(a[property] > b[property]) || -1;
      };
    },
    getRatingValue(value) {
      this.selectedRating = value;
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
          _this.hasApiCallError = true;
          console.log(error);
        });
    },
    getMovieGenresAndMovies() {
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
          _this.getPopularMovies();
        })
        .catch(function(error) {
          _this.hasApiCallError = true;
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

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
