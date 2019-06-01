<template>
  <ul class="movie-listing list-unstyled">
    <MovieCard
      v-for="movie in movieList"
      v-bind:key="movie.id"
      v-bind:title="movie.title"
      v-bind:posterUrl="movie.poster_path"
      v-bind:popularity="movie.popularity"
    />
  </ul>
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
