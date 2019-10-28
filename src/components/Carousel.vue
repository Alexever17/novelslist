<template>
  <div class="uk-position-relative uk-light uk-slider uk-slider-container" uk-slider="autoplay: true; autoplay-interval: 4000" id="carousel">
    <ul
      class="uk-slider-items uk-child-width-1 uk-child-width-1-1@s uk-child-width-1-2@m uk-child-width-1-3@l uk-child-width-1-4@xl uk-grid"
      style="transform: translateX(0px);"
    >

      <!-- Entries into the Carousel -->
      <li class="sliderParent" v-for="novel in recentNovels()" v-bind:key="novel.id">
        <div class="novel">
          <h4 class="title uk-heading-line uk-text-center carouselTitle">{{novel.Title}}</h4>
          <div>
            <img :src="novel.Picture" :alt="novel.Title + ' Cover'" class="cover">
            <h5 class="carouselRating">{{'Rating: ' + novel.Rank + '/10'}}</h5>
            <button class="uk-button uk-button-primary modalButton">More Information</button>
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
        ></polyline>
      </svg>
    </a>
    <a
      class="uk-position-center-right uk-position-small arrow uk-slidenav-next uk-icon uk-slidenav"
      href="#"
      uk-slidenav-next
      uk-slider-item="next"
    >
      <svg width="14" height="24" viewBox="0 0 14 24" xmlns="http://www.w3.org/2000/svg" ratio="1">
        <polyline fill="none" stroke="#000" stroke-width="1.4" points="1.225,23 12.775,12 1.225,1 "></polyline>
      </svg>
    </a>

    <!-- outside navigation for the slider AKA THE DOTS -->
    <ul class="uk-slider-nav uk-dotnav uk-flex-center uk-margin" id="dotnav"></ul>
  </div>
</template>

<script>
export default {
  name: "Carousel",
  props: { novels: Array },
  methods: {
    recentNovels() {
      let recent = this.novels.slice(0, 8);
      return (recent ? recent : null);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#carousel {
  margin: 0em 1.5em 1.5em 1.5em;
  background: rgba(57, 0, 90, 0.514);
  max-height: 600px;
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
