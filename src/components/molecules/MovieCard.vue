<template>
  <li>
    <img
      v-if="posterUrl"
      v-bind:src="`https://image.tmdb.org/t/p/w200/${posterUrl}`"
      v-bind:alt="`${title} poster`"
    />
    <h2>{{ title }}</h2>
    <ul v-if="movieGenres.length">
      <li 
        v-for="(genre, index) in movieGenres"
        v-bind:key="index">
        {{ genre }}
      </li>
    </ul>
  </li>
</template>

<script>
export default {
  props: {
    title: String,
    posterUrl: String,
    movieGenres: Array,
    popularity: Number
  }
};
</script>
