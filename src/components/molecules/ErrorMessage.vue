<template>
  <div class="error-msg">
    <div>
      <h2>{{ copy }}</h2>
      <img
        class="img-fluid"
        v-bind:src="gif"
        alt="sad annimated gif"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: { copy: String, gif: String }
};
</script>

<style lang="scss" scoped>
.error-msg {
  display: flex;
  justify-content: center;

  > div {
    background-color: color(secondary, contrast);
    color: color(secondary);
    text-align: center;
    border-radius: 20px;
    padding: spacer(3);
    margin-top: spacer(4);
  }

  h2 {
    margin-top: 0;
  }

  img {
    border-radius: 20px;
  }
}
</style>
