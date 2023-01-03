<template>
    <div id="app">
        <main>
            <Nav />
            <Carousel :novels="novels" />
            <NovelsList :novels="novels" />
            <Explainer :novels="novels" />
            
        </main>
        <!-- aside to scroll to the top from the bottom of the apge -->
        <a
            href="#"
            style="uk-button uk-button-default"
            id="upButton"
            uk-scroll
            uk-totop
            >Scroll Up</a
        >
        <!-- first time user get a cookie warning -->
        <footer>
            <cookie-law
                theme="dark-lime"
                message="Novelslist.com does not use cookies or the information
                    derived from them for any means. But to secure the site and
                    enable better expierence we use Cloudflare which generates
                    cookies. Also because this site contains information about
                    novels it uses images from other websites, which set
                    cookies. If you dislike that please disable third party
                    cookies for your browser."
            >
            </cookie-law>
        </footer>
    </div>
</template>

<script>
import UIkit from "uikit";
import Icons from "uikit/dist/js/uikit-icons";
UIkit.use(Icons);
import CookieLaw from "vue-cookie-law";
import Nav from "./components/Nav.vue";
import Explainer from "./components/Explainer.vue";
import Carousel from "./components/Carousel.vue";
import NovelsList from "./components/NovelsList.vue";
//for mass upload
import data from "./assets/data/novels.json";

export default {
    name: "app",
    components: {
        Nav,
        Explainer,
        Carousel,
        NovelsList,
        CookieLaw,
    },
    data() {
        return {
            novels: [{}],
        };
    },
    mounted() {
        this.novels = data.sort(function(a, b) {
            var aa = a.Date.split(".")
                    .reverse()
                    .join(""),
                bb = b.Date.split(".")
                    .reverse()
                    .join("");
            return aa < bb ? 1 : aa > bb ? -1 : 0;
        });
        document.onreadystatechange = () => {
            if (document.readyState == "complete") {
                console.log("Page completed with image and files!");
                console.log(window.cookie);
            }
        };
    },
    beforeCreate() {
    },
};
</script>

<style lang="less">
@import "../node_modules/uikit/src/less/uikit.less";
@import "./assets/less/component-stylesheets/theme.less";
// personal main less/css file
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

//changing the cookiewarning button
.Cookie__button {
    background: @mainColor !important;
}
</style>
