<template>
  <div
    id="movieListingApp"
    v-bind:class="[
      isDarkTheme ? 'is-dark-theme' : '',
      `is-theme-${pickedTheme}`
    ]"
  >
    <div class="page-wrapper">
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
    <AppFooter>
      <UiControls
        v-on:onChangeIsDark="getIsDarkValue"
        v-on:onChangePickedTheme="getPickedTheme"
        v-bind:themes="themes"
      />
    </AppFooter>
  </div>
</template>

<script>
import AppFooter from "./components/organisms/AppFooter";
import AppHeader from "./components/organisms/AppHeader";
import ErrorMessage from "./components/molecules/ErrorMessage";
import FilterBar from "./components/organisms/FilterBar";
import LoadingSpinner from "./components/molecules/LoadingSpinner";
import MovieListing from "./components/organisms/MovieListing";
import UiControls from "./components/molecules/UiControls";
import axios from "axios";
import cssVars from "css-vars-ponyfill";

export default {
  name: "app",
  components: {
    AppFooter,
    AppHeader,
    ErrorMessage,
    FilterBar,
    LoadingSpinner,
    MovieListing,
    UiControls
  },
  data() {
    return {
      movies: [],
      movieGenres: {},
      country: "GB", // Todo implement country picker
      selectedRating: 0,
      hasApiCallError: false,
      apiCallErrorMsg: "",
      isDarkTheme: false,
      themes: ["Avenger", "Sin City", "Pulp Fiction", "Alien"],
      pickedTheme: "avenger"
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
          
          results.forEach(result => {
            result.genres = result.genre_ids.map(id => _this.movieGenres[id]);
          });

          _this.movies = results.sort(_this.sortObjectBy("popularity"))
            .reverse(); // Output films from low to high popularity
        })
        .catch(function(err) {
          console.log(err)
          _this.hasApiCallError = true;
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
        .catch(function() {
          _this.hasApiCallError = true;
        });
    },
    getIsDarkValue(value) {
      this.isDarkTheme = value;
    },
    getPickedTheme(value) {
      let theme = value.replace(/\s+/g, "");
      this.pickedTheme = theme.toLowerCase();
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Lato:400,400i,700|Righteous&display=swap");

#movieListingApp {
  background-color: color(secondary, dark);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
