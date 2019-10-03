<template>
  <div id="app">
    <app-slider></app-slider>
  </div>
</template>

<script>
import Slider from "./components/Slider.vue";

export default {
  components: {
    appSlider: Slider
  }
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}

html {
  box-sizing: border-box;
  font-size: 62.5%;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif, sans-serif;
}
</style>
