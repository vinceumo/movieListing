<template>
  <div class="container">
    <transition name="fade">
      <div v-if="movieHasNoResults">
        <ErrorMessage
          copy="No results 😢"
          gif="https://media.giphy.com/media/d2lcHJTG5Tscg/giphy.gif"
        />
      </div>
    </transition>
    <transition-group tag="ul" class="movie-listing list-unstyled" name="fade">
      <MovieCard
        v-for="movie in movieListToShow()"
        v-bind:key="movie.id"
        v-bind:title="movie.title"
        v-bind:posterUrl="movie.poster_path"
        v-bind:movieGenres="movie.genres"
      />
    </transition-group>
  </div>
</template>

<script>
import ErrorMessage from "../molecules/ErrorMessage";
import MovieCard from "../molecules/MovieCard";

export default {
  components: {
    ErrorMessage,
    MovieCard
  },
  props: {
    movieList: Array,
    selectedRating: Number
  },
  data() {
    return {
      movieHasNoResults: false
    };
  },
  methods: {
    movieListToShow() {
      if (this.selectedRating === 0) {
        this.movieHasNoResults = !(this.movieList.length > 0);
        return this.movieList;
      } else {
        let movies = [];
        for (let movie of this.movieList) {
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
@supports (display: grid) {
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
}

@mixin movieListingGridFallback() {
  .movie-listing {
    display: flex;
    flex-wrap: wrap;
    margin-right: (spacer(3, true) * -1) / 2;
    margin-left: (spacer(3, true) * -1) / 2;

    > li {
      flex: 0 0 percentage(1 / 1);
      max-width: percentage(1 / 1);
      min-width: percentage(1 / 1);
      min-height: 1px;
      padding-right: spacer(3, true) / 2;
      padding-left: spacer(3, true) / 2;
      margin-bottom: spacer(3, true);

      @include min(bp(md)) {
        flex: 0 0 percentage(1 / 2);
        max-width: percentage(1 / 2);
        min-width: percentage(1 / 2);
      }

      @include min(bp(lg)) {
        flex: 0 0 percentage(1 / 3);
        max-width: percentage(1 / 3);
        min-width: percentage(1 / 3);
      }

      @include min(bp(xl)) {
        flex: 0 0 percentage(1 / 4);
        max-width: percentage(1 / 4);
        min-width: percentage(1 / 4);
      }
    }
  }
}

@supports not (display: grid) {
  @supports (display: flex) {
    @include movieListingGridFallback();
  }
}

@include ie11() {
  @include movieListingGridFallback();
}
</style>
