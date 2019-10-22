<template>
  <div id="app">
    <Nav/>
    <Explainer/>
    <Carousel :novels=novels />
  </div>
</template>

<script>
import UIkit from "uikit";
import Icons from "uikit/dist/js/uikit-icons";
UIkit.use(Icons);
import Nav from "./components/Nav.vue";
import Explainer from "./components/Explainer.vue";
import Carousel from "./components/Carousel.vue";
import data from "./assets/data/data.json"

export default {
  name: "app",
  components: {
    Nav, Explainer, Carousel
  },
  data() {
    return {
      novels: data
    }
  }
};
</script>

<style lang="less">
@import "../node_modules/uikit/src/less/uikit.less";
@import "./assets/less/theme.less";
#app {
  background-image: url("assets/images/pipes.png");
  background-size: 6em 6em;
  padding-bottom: 5em; 
}
</style>
