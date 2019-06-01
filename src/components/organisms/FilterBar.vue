<template>
  <div class="container filter-bar">
    <label for="ratingFilter">Filter by ratings above: </label>
    <select id="ratingFilter" v-on:change="emitRatingValue">
      <option v-for="n in 11" v-bind:key="n - 1" v-bind:value="n - 1">
        {{ n - 1 }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  methods: {
    emitRatingValue(event) {
      const value = event.target.value;
      this.$emit("ratingValChange", value);
    }
  }
};
</script>

<style lang="scss" scoped>
.filter-bar {
  background-color: color(tertiary, dark);
  color: color(tertiary, contrast);
  padding: spacer(2);
  margin-bottom: spacer(3);
}
</style>
