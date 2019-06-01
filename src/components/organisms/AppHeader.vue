<template>
  <header class="container">
    <h1>{{ title }}</h1>
  </header>
</template>

<script>
export default {
  props: {
    title: String
  }
};
</script>

<style lang="scss" scoped>
header {
  text-align: center;
  padding-top: spacer(4);
  padding-bottom: spacer(4);

  @include min(bp(md)) {
    padding-top: spacer(6);
    padding-bottom: spacer(6);
  }

  h1 {
    color: color(primary);
  }
}
</style>
