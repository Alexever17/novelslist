<template>
  <div id="explain">
    <div class="uk-flex uk-flex-center">
      <div class="uk-child-width-1-2@m uk-grid-small uk-grid-match" uk-grid>
        <div>
          <div class="uk-card uk-card-default uk-card-body">
            <article class="uk-comment">
              <header class="uk-comment-header uk-grid-medium uk-flex-middle uk-grid" uk-grid>
                <div class="uk-width-auto uk-first-column">
                  <!-- admins profile pic -->
                  <img
                    class="uk-comment-avatar"
                    src="https://pbs.twimg.com/profile_images/790276964049362944/MXjuA9bL_400x400.jpg"
                    width="80"
                    height="80"
                    alt
                  />
                </div>
                <!-- the message itself -->
                <div class="uk-width-expand">
                  <h4 class="uk-comment-title uk-margin-remove">
                    <a class="uk-link-reset" href="#">Alex</a>
                  </h4>
                  <ul class="uk-comment-meta uk-subnav uk-subnav-divider uk-margin-remove-top">
                    <li>
                      <a href="#" id="timeStamp">{{timeNumber()}} days ago</a>
                    </li>
                  </ul>
                </div>
              </header>
              <div class="uk-comment-body">
                <h3 class="uk-card-title">Welcome!</h3>
                <p>I love reading web novels and want to share my passion with you. On this website you can find all novels I ever read. Please scroll down to see my most favorite ones.</p>
              </div>
            </article>
          </div>
        </div>
        <div>
          <div class="uk-card uk-card-default uk-card-body">
            <h3 class="uk-card-title">Where can I read novels?</h3>
            <p>Sometimes it is difficult to find the most interesting novels, because many are translated by fans and aren't released officially. There are multiple sources to find novels but two of the most famous are novelupdates.com and webnovel.com. Novelupdates is a website which tracks all translated novels and has their information, but does not display the recent chapters for paid chapters. But you can find the licensing information. Webnovel meanwhile is the biggest plattform for translated and original novels.</p>
            <a href="https://novelupdates.com">Novel Updates</a> <br>
            <a href="https://webnovel.com">Webnovel</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Explainer",
  props: {},
  methods: {
    timeNumber() {
      const date1 = new Date(2019, 9, 19, 20, 0);
      const date2 = new Date();
      let days = Math.round((date2 - date1) / 1000 / 60 / 60 / 24)
      return days;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#explain {
  margin: 0em 1em 1em 1em;
}
</style>