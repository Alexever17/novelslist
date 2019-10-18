<template>
  <div id="start">
    
    <navbar>
      <div class="uk-position-relative">
        <img
          src="../assets/images/alejandro-escamilla-cZhUxIQjILg-unsplash.jpg"
          alt="a book picture"
        />
        <div class="uk-position-top">
          <nav class="uk-navbar-container uk-navbar-transparent" uk-navbar>
            <div class="uk-navbar-center">
              <a class="uk-navbar-item uk-logo" href="#">
                <img src="../assets/images/logor.png" alt id="centerlogo" />
              </a>
            </div>
          </nav>
        </div>
      </div>
    </navbar>
    
  </div>
</template>

<script>
// import axios from "axios";
export default {
  name: "Main",
  props: {
    msg: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#centerlogo {
  min-height: 100px;
  max-height: 20vh;
}
</style>
