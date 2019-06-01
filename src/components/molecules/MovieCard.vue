<template>
  <li>
    <img
      v-if="posterUrl"
      v-bind:src="`https://image.tmdb.org/t/p/w200/${posterUrl}`"
      v-bind:alt="`${title} poster`"
    />
    <h2>{{ title }}</h2>
  </li>
</template>

<script>
export default {
  props: {
    title: String,
    posterUrl: String,
    genders: Array,
    popularity: Number
  }
};
</script>
