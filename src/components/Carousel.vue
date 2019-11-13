<template>
  <div
    class="uk-position-relative uk-light uk-slider uk-slider-container"
    uk-slider="autoplay: true; autoplay-interval: 6000"
    id="carousel"
  >
    <ul
      class="uk-slider-items uk-child-width-1 uk-child-width-1-1@s uk-child-width-1-2@m uk-child-width-1-3@l uk-child-width-1-4@xl uk-grid"
      style="transform: translateX(0px);"
    >
      <!-- Entries into the Carousel -->
      <li class="sliderParent" v-for="(novel, index) in carouselData" v-bind:key="novel.id">
        <div class="novel">
          <h3 class="title uk-heading-line uk-text-center carouselTitle">{{novel.Title}}</h3>
          <div>
            <img rel="preconnect" :src="novel.Picture" :alt="novel.Title + ' Cover'" class="cover" />
            <h5 class="carouselRating">{{'Rating: ' + novel.Rank + '/10'}}</h5>

            <!-- This is a button toggling the modal with the default close button -->
            <button
              class="uk-button uk-button-primary"
              type="button"
              :uk-toggle="'target: #' + 'modal' + modalIdMaker(novel.Title)"
            >More Info</button>

           <Modal :novel="novel" :id="'modal' + modalIdMaker(novel.Title)" uk-modal/>

          </div>
        </div>
      </li>
    </ul>

    <!-- Slider arrows inside the slider -->
    <a
      class="uk-position-center-left uk-position-small arrow uk-slidenav-previous uk-icon uk-slidenav"
      href="#"
      uk-slidenav-previous
      uk-slider-item="previous"
    >
      <svg width="14" height="24" viewBox="0 0 14 24" xmlns="http://www.w3.org/2000/svg" ratio="1">
        <polyline
          fill="none"
          stroke="#000"
          stroke-width="1.4"
          points="12.775,1 1.225,12 12.775,23 "
        />
      </svg>
    </a>
    <a
      class="uk-position-center-right uk-position-small arrow uk-slidenav-next uk-icon uk-slidenav"
      href="#"
      uk-slidenav-next
      uk-slider-item="next"
    >
      <svg width="14" height="24" viewBox="0 0 14 24" xmlns="http://www.w3.org/2000/svg" ratio="1">
        <polyline
          fill="none"
          stroke="#000"
          stroke-width="1.4"
          points="1.225,23 12.775,12 1.225,1 "
        />
      </svg>
    </a>

    <!-- outside navigation for the slider AKA THE DOTS -->
    <ul class="uk-slider-nav uk-dotnav uk-flex-center uk-margin" id="dotnav"></ul>
  </div>
</template>

<script>
import Modal from "./Modal.vue";

export default {
  name: "Carousel",
  props: { novels: Array },
  components: {Modal},
  watch: {
    // whenever novels changes, this function will run
    novels: function(newVal, oldVal) {
      this.carouselData = newVal.slice(0, 8);
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
#carousel {
  margin: 0em 2em 2em 2em;
  background: linear-gradient(
    90deg,
    rgba(57, 0, 90, 0.6),
    rgba(57, 0, 90, 0.5),
    rgba(57, 0, 90, 0.6)
  );
  max-height: 600px;
  border: 1px solid #4d4e5234;
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
