<template>
  <div id="list">
    <p>TEST</p>
    <p>{{ getNovels() }}</p>
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

export default {
  name: "List",
  props: {},
  methods: {
    getNovels() {
      var db = firebase.firestore();

      db.collection("novels")
        .get()
        .then(function(querySnapshot) {
          querySnapshot.forEach(function(doc) {
            // doc.data() is never undefined for query doc snapshots
            console.log(doc.id, " => ", doc.data())
            console.log(doc.data().Description)
            return doc.data().Description
          });
        })
        .catch(function(error) {
          return ("Error getting documents");
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#list {
  margin: 0em 1em 1em 1em;
}
</style>
