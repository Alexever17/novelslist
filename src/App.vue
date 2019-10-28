<template>
  <div id="app">
    <Nav />
    <Explainer />
    <Carousel :novels="novels" />
    <NovelsList :novels="novels" :columns="columns"/>
  </div>
</template>

<script>
import UIkit from "uikit";
import Icons from "uikit/dist/js/uikit-icons";
UIkit.use(Icons);
import Nav from "./components/Nav.vue";
import Explainer from "./components/Explainer.vue";
import Carousel from "./components/Carousel.vue";
import NovelsList from "./components/NovelsList.vue";
import data from "./assets/data/csvjson.json";

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
    NovelsList
  },
  data() {
    return {
      novels: [{}],
      columns: [
        {
          label: "Picture",
          field: "Picture",
          width: '90px'
        },
        {
          label: "Title",
          field: "Title",
          width: '300px'
        },
        {
          label: "Rank",
          field: "Rank",
          type: "number",
          width: '30px'
        },
        {
          label: "Added",
          field: "Date",
          type: "date",
          dateInputFormat: "dd.MM.yyyy",
          dateOutputFormat: "dd.MM.yyyy",
          hidden: true,
        },
        {
          label: "Description",
          field: "Description"
        },
        {
          label: "Dropped",
          field: "Dropped",
          type: "boolean",
          width: '55px'
        },
        {
          label: "Origin",
          field: "Origin",
          filterable: true,
          width: '80px'
        },
        {
          label: "Link",
          field: "Link",
          width: '70px'
        },
        {
          label: "Progress",
          field: "Progress",
          hidden: true,
        },      
      ]
    };
  },
  mounted() {
    db.collection("novels")
      .get()
      .then(query => {
        var result = query.docs.map(x => x.data());
        this.novels = result;
      })
      .catch(function(error) {
        this.novels = "Error getting documents: " + error;
      });
  },
  methods: {
    // cool mass upload!

    // upload() {
    //   console.log(data);
      
    //   data.map(novel => {
    //     db.collection("novels")
    //       .add(novel)
    //       .then(function(docRef) {
    //         console.log("Document written with ID: ", docRef.id);
    //       })
    //       .catch(function(error) {
    //         console.error("Error adding document: ", error);
    //       });
    //   });
    // }
  }
};
</script>

<style lang="less">
@import "../node_modules/uikit/src/less/uikit.less";
@import "./assets/less/theme.less";
#app {
  background-image: url("assets/images/pipes.png");
  background-size: 8em 8em;
  padding-bottom: 5em;
}
</style>
