<template>
  <div id="carousel">
    <div class="uk-position-relative uk-visible-toggle uk-light" tabindex="-1" uk-slideshow>
      <!-- <ul class="uk-slideshow-items" id="newEntries">
        <li v-for="item in items">{{ item.message }}</li>
      </ul> -->

      <a
        class="uk-slidenav-large uk-position-center-left uk-position-small uk-hidden-hover"
        href="#"
        uk-slidenav-previous
        uk-slideshow-item="previous"
      ></a>
      <a
        class="uk-slidenav-large uk-position-center-right uk-position-small uk-hidden-hover"
        href="#"
        uk-slidenav-next
        uk-slideshow-item="next"
      ></a>
    </div>
  </div>
</template>

<script>
//import novels from "../assets/data/data.js";

export default {
  name: "Carousel",
  props: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#carousel {
  margin-bottom: 1em;
}
</style>
