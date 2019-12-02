<template>
<!-- container for the slider with title and the slider -->
  <div id="carouselContainer">
    <!-- title of the container -->
    <div class="uk-card uk-card-default uk-card-body border" id="carouselTitle">
      <ul class="uk-flex-left" uk-tab>
        <li class="uk-active">
          <h2 class="carouselTitleH2">Newest Additions</h2>
        </li>
      </ul>
    </div>
    <!-- slider start -->
    <div
      class="uk-position-relative uk-light uk-slider uk-slider-container"
      uk-slider="autoplay: true; autoplay-interval: 12000; sets: true"
      id="carousel"
    >
    <!-- ul which holds the entries -->
      <ul
        class="uk-slider-items uk-child-width-1 uk-child-width-1-1@s uk-child-width-1-2@m uk-child-width-1-3@l uk-child-width-1-4@xl uk-grid"
        style="transform: translateX(0px);"
      >
        <!-- Entries into the Carousel, uses vues v-for method to 
        generate entries taking the data from the carouselData -->
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

              <!-- This is a button toggling the modal -->
              <!-- modealIdMaker transforms a novels title to create a valide id -->
              <button
                class="uk-button uk-button-primary"
                type="button"
                :uk-toggle="'target: #' + 'modal' + modalIdMaker(novel.Title)"
              >
                More Info
              </button>
              <!-- Modal with more information, generated for each carousel component -->
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
    // necessary to detect api call
    novels: function(newVal, oldVal) {
      this.carouselData = newVal.slice(0, 12);
    }
  },
  data() {
    return {
      //starts with some placeholder novels just to bridge the api call
      carouselData: variables.placeholderNovels
    };
  },
  methods: {
    //transforms a novels title to create a valide id
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

//title on top of the slider
#carouselTitle {
  display: flex;
  padding: 20px 0 20px 0;
  justify-content: center;
  //the text of the box
  .carouselTitleH2 {
    margin: 0;
    padding: 0;
  }
}

//border for the carouselTitle
.border {
  border: 1px solid @borderColor;
  border-width: 1px 1px 0px 1px;
}

//slider itself
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

//each slider element has this wrapper
.sliderParent {
  text-align: center;
}

//the title of the novel
.carouselTitle {
  margin: 5px 5px;
  height: 60px;
  vertical-align: middle;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

//picture in the slider
.cover {
  height: 400px;
  width: 300px;
}

//carousel rating
.carouselRating {
  margin: 10px 0 15px 0;
}
</style>
