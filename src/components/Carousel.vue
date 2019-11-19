<template>
  <div
    class="uk-position-relative uk-light uk-slider uk-slider-container"
    uk-slider="autoplay: true; autoplay-interval: 12000; sets: true"
    id="carousel"
  >
    <ul
      class="uk-slider-items uk-child-width-1 uk-child-width-1-1@s uk-child-width-1-2@m uk-child-width-1-3@l uk-child-width-1-4@xl uk-grid"
      style="transform: translateX(0px);"
    >
      <!-- Entries into the Carousel -->
      <li
        class="sliderParent"
        v-for="(novel, index) in carouselData"
        v-bind:key="novel.id"
      >
        <div class="novel">
          <h3 class="title uk-heading-line uk-text-center carouselTitle">
            {{ novel.Title }}
          </h3>
          <div>
            <img
              rel="preconnect"
              :src="novel.Picture"
              :alt="novel.Title + ' Cover'"
              class="cover"
            />
            <h5 class="carouselRating">
              {{ "Rating: " + novel.Rank + "/10" }}
            </h5>

            <!-- This is a button toggling the modal with the default close button -->
            <button
              class="uk-button uk-button-primary"
              type="button"
              :uk-toggle="'target: #' + 'modal' + modalIdMaker(novel.Title)"
            >
              More Info
            </button>

            <Modal
              :novel="novel"
              :id="'modal' + modalIdMaker(novel.Title)"
              uk-modal
            />
          </div>
        </div>
      </li>
    </ul>

    <!-- outside navigation for the slider AKA THE DOTS -->
    <ul
      class="uk-slider-nav uk-dotnav uk-flex-center uk-margin"
      id="dotnav"
    ></ul>
  </div>
</template>

<script>
import Modal from "./Modal.vue";

export default {
  name: "Carousel",
  props: { novels: Array },
  components: { Modal },
  watch: {
    // whenever novels changes, this function will run
    novels: function(newVal, oldVal) {
      this.carouselData = newVal.slice(0, 12);
    }
  },
  data() {
    return {
      carouselData: []
    };
  },
  methods: {
    modalIdMaker(input) {
      return input.replace(/\W+/g, "");
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@import "../assets/less/main.less";

#carousel {
  margin: 0em 2em 2em 2em;
  background: linear-gradient(
    90deg,
    @mainColorLight,
    fadeout(@mainColor, 50%),
    @mainColorLight
  );
  max-height: 600px;
  border: 1px solid @borderColor;
}
.sliderParent {
  text-align: center;
}
.carouselTitle {
  margin: 5px 0;
  height: 60px;
  vertical-align: middle;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.cover {
  height: 400px;
  width: 300px;
}
.carouselRating {
  margin: 10px 0 15px 0;
}
</style>
