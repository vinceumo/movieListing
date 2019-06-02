<template>
  <li class="movie-card" v-bind:class="posterUrl ? 'has-poster' : ''">
    <img
      v-if="posterUrl"
      v-bind:src="`https://image.tmdb.org/t/p/w200/${posterUrl}`"
      v-bind:alt="`${title} poster`"
    />
    <div class="movie-card-content">
      <h2>{{ title }}</h2>
      <ul class="tags" v-if="movieGenres.length">
        <li v-for="(genre, index) in movieGenres" v-bind:key="index">
          {{ genre }}
        </li>
      </ul>
      <!-- <button class="btn is-tertiary">More info<span class="screen-reader-only">about {{title}}</span></button> -->
    </div>
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

<style lang="scss" scoped>
.is-dark-theme {
  .movie-card {
    background-color: color(secondary);

    h2 {
      color: color(secondary, contrast);
    }

    &.has-poster {
      img {
        filter: brightness(80%);
      }
      .movie-card-content {
        h2 {
          background-color: color(secondary);
        }
      }
    }
  }
}

.movie-card {
  background-color: color(secondary, contrast);
  padding: spacer(3);
  border-radius: 20px;
  display: flex;
  align-items: flex-start;

  &.has-poster {
    .movie-card-content {
      margin-left: spacer(2);
      width: 100%;
      position: relative;

      h2 {
        background-color: color(secondary, contrast);
        padding: 0 0.45rem 0 0.45rem;
        line-height: 1.25;
        margin-left: -2rem;
        //transform: translateX(-2rem);
      }
    }
  }

  img {
    height: auto;
    max-width: 100%;
    width: 100px;
    border-radius: 20px;

    @include ie11() {
      max-width: 100px;
      min-width: 100px;
    }

    @include min(bp(md)) {
      width: 150px;

      @include ie11() {
        max-width: 150px;
        min-width: 150px;
      }
    }
  }

  h2 {
    margin-top: 0;
  }

  .tags {
    list-style-type: none;
    padding-left: 0;
    display: flex;
    flex-wrap: wrap;
    margin-bottom: spacer(3);

    > li {
      background-color: color(primary, light);
      padding: spacer(1);
      margin-right: spacer(2);
      margin-bottom: spacer(1);
      font-style: italic;

      &:nth-last-child(1) {
        margin-right: 0;
      }
    }
  }
}
</style>
