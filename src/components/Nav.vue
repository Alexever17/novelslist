<template>
  <div id="navContainer">
    <!-- desktop navigation -->
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
    <!-- mobile navigation -->
    <nav class="uk-navbar-container" uk-navbar="mode: click" id="navMobile">
      <div class="uk-navbar-left">
        <ul class="uk-navbar-nav">
          <li class="uk-navbar-item uk-logo">
            <img src="../assets/images/logor.66ec5d99.png" alt id="centerlogo">
          </li>
        </ul>
        <ul class="uk-navbar-nav uk-position-right">
          <li class="uk-flex uk-flex-center uk-flex-middle uk-navbar-item">
            <button href="#offcanvas-slide" class="navbarLink uk-button uk-button-primary mainColorBackground" uk-toggle>Menu</button>
          </li>
        </ul>
        <!-- there is an offcanvas feature in uikit which is perfect to create mobile menus -->
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
// desktop nav, hidden on mobile
#nav {
  max-height: 250px;
  padding-left: 1em;
  .over1000();
  .backgroundImg();
}
// mobile nav, hidden on desktop
#navMobile {
  .under1000();
  .backgroundImg();
}

//color change and slight changes to the menu button from mobile nav
.mainColorBackground, .mainColorBackground:hover, .mainColorBackground:focus {
  padding: 0px 30px;
  background-color: @mainColor;
  border-color: rgb(172, 172, 172);
  color: white;
}

//nav buttons in both versions
.navbarLink {
  font-size: 150%;
}

//logo
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

//changes for the mobile version
@media screen and (max-width: 900px) {
  .navbarLink {
    font-size: 100%;
  }
  #centerlogo {
    max-height: 50px;
  }
}
</style>
