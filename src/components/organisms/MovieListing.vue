<template>
  <div class="container">
    <ul class="movie-listing list-unstyled">
      <MovieCard
        v-for="movie in movieList"
        v-bind:key="movie.id"
        v-bind:title="movie.title"
        v-bind:posterUrl="movie.poster_path"
        v-bind:movieGenres="movie.genres"
      />
    </ul>
  </div>
</template>

<script>
import MovieCard from "../molecules/MovieCard.vue";

export default {
  components: {
    MovieCard
  },
  props: {
    movieList: Array
  }
};
</script>

<style lang="scss" scoped>
.movie-listing {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  grid-gap: spacer(3);

  @include min(bp(sm)) {
    grid-gap: spacer(4);
  }

  @include min(bp(md)) {
    grid-gap: spacer(5);
  }
}
</style>
