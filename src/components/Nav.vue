<template>
  <div id="nav">
    <navbar>
      <div class="uk-position-relative">
        <img
          src="../assets/images/danny-zjV8ptYgcEo-unsplash.jpg"
          alt="a book picture"
        />
        <div class="uk-position-top">
          <nav class="uk-navbar-container uk-navbar-transparent" uk-navbar>
            <div class="uk-navbar-center">
              <ul class="uk-navbar-nav">
                <li>
                  <a href="#">Item</a>
                </li>
                <li class="uk-navbar-item uk-logo">
                    <img src="../assets/images/logor.png" alt id="centerlogo" />
                </li>
                <li>
                  <a href="#">Item</a>
                </li>
              </ul>
            </div>
          </nav>
        </div>
      </div>
    </navbar>
  </div>
</template>

<script>
export default {
  name: "Nav",
  props: {
    
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#centerlogo {
  min-height: 100px;
  max-height: 15vh;
}
#nav {
  margin-bottom: 1em;
}
</style>
