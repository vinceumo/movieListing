<template>
  <footer>
    <div class="container">
      <ul class="links list-unstyled">
        <li><a href="https://vincent-humeau.com">vincent-humeau.com</a></li>
        <li><a href="https://github.com/vinceumo/movieListing">Github</a></li>
      </ul>
      <slot></slot>
    </div>
  </footer>
</template>

<style lang="scss" scoped>
footer {
  background-color: color(secondary);
  color: color(secondary, contrast);
  padding: spacer(5) 0;
  margin-top: spacer(6);

  a {
    color: color(secondary, contrast);
  }

  .links {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;

    > li {
      margin: spacer(1) spacer(2);
    }
  }
}
</style>
