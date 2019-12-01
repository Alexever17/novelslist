<template>
  <div id="navContainer">
    <nav class="uk-navbar-container" uk-navbar="mode: click" id="nav">
      <div class="uk-navbar-left">
        <ul class="uk-navbar-nav">
          <li class="uk-navbar-item uk-logo">
            <img src="../assets/images/logor.66ec5d99.png" alt id="centerlogo">
          </li>
          <li>
            <a href="#list" uk-scroll class="navbarLink">Full List</a>
          </li>
          <li>
            <a href="https://github.com/Alexever17" target="_blank" class="navbarLink">Creator</a>
          </li>
        </ul>
      </div>
    </nav>
    <nav class="uk-navbar-container" uk-navbar="mode: click" id="navMobile">
      <div class="uk-navbar-left">
        <ul class="uk-navbar-nav">
          <li class="uk-navbar-item uk-logo">
            <img src="../assets/images/logor.66ec5d99.png" alt id="centerlogo">
          </li>
          <li>
            <a href="#offcanvas-slide" class="navbarLink" uk-toggle>Menu</a>
          </li>
        </ul>
        <div id="offcanvas-slide" uk-offcanvas>
          <div class="uk-offcanvas-bar">
            <ul class="uk-nav uk-nav-default">
              <li class="uk-navbar-item uk-logo">
                <img src="../assets/images/logo.png" alt id="centerlogo">
              </li>
              <li>
                <a href="#list" uk-scroll class="navbarLink">Full List</a>
              </li>
              <li>
                <a href="https://github.com/Alexever17" target="_blank" class="navbarLink">Creator</a>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "Nav",
  props: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@import "../assets/less/main.less";
#navContainer {
  width: 100%;
}
.backgroundImg {
  background-image: url("../assets/images/danny-zjV8ptYgcEo-unsplash.7573259d.jpg");
  background-position: center; /* Center the image */
  background-repeat: no-repeat; /* Do not repeat the image */
  background-size: cover;
}
#nav {
  max-height: 250px;
  padding-left: 1em;
  .over1000();
  .backgroundImg();
}
#navMobile {
  .under1000();
  .backgroundImg();
}
#centerlogo {
  min-height: 100px;
  max-height: 120px;
}
//because of the picture the li elements are stretched large, so hover responds even when not directly on the a element
//this fixes it
.uk-navbar-nav > li:hover > a {
  color: #999;
}
.uk-navbar-nav > li:hover > a:hover,
.uk-navbar-nav > li > a:focus {
  color: #666;
}
.navbarLink {
  font-size: 200%;
}
@media screen and (max-width: 900px) {
  .navbarLink {
    font-size: 100%;
  }
  #centerlogo {
    max-height: 50px;
  }
}
</style>
