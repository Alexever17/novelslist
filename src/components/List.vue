<template>
  <div id="list">
    <p>TEST</p>
    <p>{{JSON.stringify(novels[0])}}</p>
  </div>
</template>

<script>
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
  name: "List",
  props: {},
  data() {
    return {
      novels: null
    };
  },
  methods: {},
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
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#list {
  margin: 0em 1em 1em 1em;
}
</style>
