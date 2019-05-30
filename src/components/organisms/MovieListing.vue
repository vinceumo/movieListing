<template>
  <ul class="movie-listing list-unstyled">
    <MovieCard title="Star Wars" />
  </ul>
</template>

<script>
import MovieCard from "../molecules/MovieCard.vue";

export default {
  components: {
    MovieCard
  }
};
</script>
