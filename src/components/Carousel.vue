<template>
  <div id="carouselContainer">
    <div class="uk-card uk-card-default uk-card-body border" id="tabsTitle">
      <ul class="uk-flex-left" uk-tab>
        <li class="uk-active">
          <h2 class="tabsTitleH2">Newest Additions</h2>
        </li>
      </ul>
    </div>
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

              <Modal :novel="novel" uk-modal />
            </div>
          </div>
        </li>
      </ul>

      <!-- outside navigation for the slider AKA THE DOTS -->
      <ul
        class="uk-slider-nav uk-dotnav uk-flex-center uk-margin focusCorrection"
        id="dotnav"
      ></ul>
    </div>
  </div>
</template>

<script>
import Modal from "./Modal.vue";
import variables from "../assets/data/variables.json";

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
      carouselData: variables.placeholderNovels
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

#carouselContainer {
  width: 100%;
  max-width: ~"calc(1800px + 4em)";
  padding: 0em 2em;
  margin: 2em 0em 0em 0em;
  box-sizing: border-box;
}

@media only screen and (max-width: 480px) {
  #carouselContainer {
    padding: 0em 0em;
    margin: 2em 0em 0em 0em;
  }
}

#tabsTitle {
  display: flex;
  padding: 20px 0 20px 0;
  justify-content: center;
  .tabsTitleH2 {
    margin: 0;
    padding: 0;
  }
}
.border {
  border: 1px solid @borderColor;
  border-width: 1px 1px 0px 1px;
}

#carousel {
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
  margin: 5px 5px;
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

.uk-light .uk-dotnav > * > :focus {
  background-color: yellow !important;
}
</style>
