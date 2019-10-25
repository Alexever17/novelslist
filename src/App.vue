<template>
  <div id="app">
    <Nav/>
    <Explainer/>
    <Carousel :novels="novels"/>
    <List/>
  </div>
</template>

<script>
import UIkit from "uikit";
import Icons from "uikit/dist/js/uikit-icons";
UIkit.use(Icons);
import Nav from "./components/Nav.vue";
import Explainer from "./components/Explainer.vue";
import Carousel from "./components/Carousel.vue";
import List from "./components/List.vue";
import data from "./assets/data/data.json";
import firebase from "firebase";

var firebaseConfig = {
  apiKey: process.env.VUE_APP_apiKey,
  authDomain: process.env.VUE_APP_authDomain,
  databaseURL: process.env.VUE_APP_databaseURL,
  projectId: process.env.VUE_APP_projectId,
  storageBucket: process.env.VUE_APP_storageBucket,
  messagingSenderId: process.env.VUE_APP_messagingSenderId,
  appId: process.env.VUE_APP_appId,
  measurementId: process.env.VUE_APP_measurementId
};

firebase.initializeApp(firebaseConfig);
firebase.analytics();

export default {
  name: "app",
  components: {
    Nav,
    Explainer,
    Carousel,
    List
  },
  data() {
    return {
      novels: data
    };
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
