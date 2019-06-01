<template>
  <div class="error-msg">
    <h2>{{ copy }}</h2>
    <img class="img-fluid" src="https://media.giphy.com/media/d2lcHJTG5Tscg/giphy.gif" alt="sad annimated gif">
  </div>
</template>

<script>
export default {
  props: {copy: String}
}
</script>

<style lang="scss" scoped>
.error-msg {
  background-color: color(secondary, contrast);
  color: color(secondary);
  text-align: center;
  border-radius: 20px;
  padding: spacer(3);
  margin-top: spacer(4);

  h2 {
    margin-top: 0;
  }

  img {
    border-radius: 20px;
  }
}
</style>
