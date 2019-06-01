<template>
  <div class="container">
    <transition-group tag="ul" class="movie-listing list-unstyled" name="fade">
      <MovieCard
        v-for="movie in movieListToShow()"
        v-bind:key="movie.id"
        v-bind:title="movie.title"
        v-bind:posterUrl="movie.poster_path"
        v-bind:movieGenres="movie.genres"
      />
    </transition-group>
    <transition name="fade">
      <div v-if="movieHasNoResults">
        <p>
          No results
        </p>
        
      </div>
    </transition>
  </div>
</template>

<script>
import MovieCard from "../molecules/MovieCard.vue";

export default {
  components: {
    MovieCard
  },
  props: {
    movieList: Array,
    selectedRating: Number
  },
  data() {
    return {
      movieHasNoResults: false
    }
  },
  methods: {
    movieListToShow() {
      if (this.selectedRating === 0) {
        this.movieHasNoResults = !(this.movieList.length > 0);
        return this.movieList;
      } else {
        let movies = [];
        for (let movie of this.movieList) {
          console.log(movie.vote_average);
          if (movie.vote_average >= this.selectedRating) movies.push(movie);
        }
        this.movieHasNoResults = !(movies.length > 0);
        return movies;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.movie-listing {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  grid-gap: spacer(3);

  @include min(bp(sm)) {
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    grid-gap: spacer(4);
  }

  @include min(bp(md)) {
    grid-gap: spacer(5);
  }
}
</style>
