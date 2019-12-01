<template>
  <div id="app">
    <main>
      <Nav />
      <Carousel :novels="novels" />
      <Explainer :novels="novels" />
      <NovelsList :novels="novels" />
    </main>
    <a
      href="#"
      style="uk-button uk-button-default"
      id="upButton"
      uk-scroll
      uk-totop
      >Scroll Up</a
    >
    <footer>
    <cookie-law theme="dark-lime"></cookie-law>
  </footer>
  </div>
</template>

<script>
import UIkit from "uikit";
import Icons from "uikit/dist/js/uikit-icons";
UIkit.use(Icons);
import CookieLaw from 'vue-cookie-law'
import Nav from "./components/Nav.vue";
import Explainer from "./components/Explainer.vue";
import Carousel from "./components/Carousel.vue";
import NovelsList from "./components/NovelsList.vue";
//for mass upload
//import data from "./assets/data/csvjson.json";

//the core of firebase
import * as firebase from "firebase/app";

// If you enabled Analytics in your project, add the Firebase SDK for Analytics
import "firebase/analytics";

// Add the Firebase products that you want to use
import "firebase/auth";
import "firebase/firestore";

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

var db = firebase.firestore();

export default {
  name: "app",
  components: {
    Nav,
    Explainer,
    Carousel,
    NovelsList,
    CookieLaw
  },
  data() {
    return {
      novels: [{}]
    };
  },
  mounted() {
    db.collection("novels")
      .orderBy("Date", "desc")
      .get()
      .then(query => {
        var result = query.docs.map(x => x.data());
        //sorting for date in the novels with the most recent first
        this.novels = result.sort(function(a, b) {
          var aa = a.Date.split(".")
              .reverse()
              .join(""),
            bb = b.Date.split(".")
              .reverse()
              .join("");
          return aa < bb ? 1 : aa > bb ? -1 : 0;
        });
      })
      .catch(function(error) {
        this.novels = "Error getting documents: " + error;
      });
  },
  beforeCreate() {
    //mass upload
    // data.map(novel => {
    //     db.collection("novels")
    //       .add(novel)
    //       .then(function(docRef) {
    //         console.log("Document written with ID: ", docRef.id);
    //       })
    //       .catch(function(error) {
    //         console.error("Error adding document: ", error);
    //       });
    //   });
  }
};
</script>

<style lang="less">
@import "../node_modules/uikit/src/less/uikit.less";
@import "./assets/less/component-stylesheets/theme.less";
@import "./assets/less/main.less";

#app {
  background-image: url("assets/images/pipes.png");
  background-size: 8em 8em;
  padding-bottom: 1.5em;
}

main {
  display: flex;
  justify-content: center;
  align-content: center;
  flex-wrap: wrap;
}

#upButton {
  position: relative;
  margin-left: ~"calc(50% - 2em)";
  margin-top: 1em;
  width: 4em;
  height: 1.5em;
  line-height: 1.5em;
  text-align: center;
  background-color: fadeout(@mainColor, 30%);
  color: white;
  border: 1px solid @borderColor;
  font-size: 150%;
  & svg {
    display: none;
  }
}

.Cookie__button {
  background: @mainColor !important;
}
</style>
