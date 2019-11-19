<template>
  <div id="explain" class="uk-flex uk-flex-center">
    <div
      class="uk-child-width-1-2@m uk-child-width-1-2@l uk-grid-small uk-grid-match uk-flex-center uk-width-1-1"
      uk-grid
    >
      <div>
        <div class="uk-card uk-card-default uk-card-body borderAddition">
          <article class="uk-comment">
            <header
              class="uk-comment-header uk-grid-medium uk-flex-middle uk-grid"
              uk-grid
            >
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
                <ul
                  class="uk-comment-meta uk-subnav uk-subnav-divider uk-margin-remove-top"
                >
                  <li>
                    <a href="#" id="timeStamp">{{ timeNumber() }} days ago</a>
                  </li>
                </ul>
              </div>
            </header>
            <div class="uk-comment-body">
              <h3 class="uk-card-title">Welcome!</h3>
              <p>
                I love reading web novels and want to share my passion with you.
                On this website you can find all novels I ever read.
                <br /><br />
                Sometimes it is difficult to find the most interesting novels,
                because many are translated by fans and aren't released
                officially. There are multiple sources to find novels but the
                two most used are novelupdates.com and webnovel.com.
                Novelupdates is a website which tracks all translated novels and
                has their information. There you can find the licensing
                information and where you can read it. Webnovel meanwhile is the
                biggest plattform for translated and original novels.
              </p>
              <a href="https://novelupdates.com">Novel Updates</a>
              <br />
              <a href="https://webnovel.com">Webnovel</a>
            </div>
          </article>
        </div>
      </div>
      <div>
        <div class="uk-card uk-card-default uk-card-body borderAddition">
          <h3 class="uk-card-title">Database Information</h3>
          <p>Click on a category to disable it from the chart or add it again.</p>
          <PieChart :options="options" :chartdata="chartdata" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PieChart from "./PieChart.vue";
import randomColor from "random-color";

export default {
  name: "Explainer",
  props: { novels: Array },
  data() {
    return {
      chartdata: {
        labels: [
          "Chinese Novels",
          "Korean Novels",
          "Japanese Novels",
          "Webnovel Originals",
          "Traditional Books",
          "Other Websites",
          "Dropped Novels"
        ],
        datasets: [
          {
            label: "Alexever17 Novels",
            backgroundColor: this.colorCreator(),
            data: [63, 15, 24, 17, 1, 2, 58]
          }
        ]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        circumference: Math.PI,
        rotation: -Math.PI
      }
    };
  },
  components: { PieChart },
  methods: {
    timeNumber() {
      const date1 = new Date(2019, 9, 19, 20, 0);
      const date2 = new Date();
      let days = Math.round((date2 - date1) / 1000 / 60 / 60 / 24);
      return days;
    },
    colorCreator() {
      let output = [];
      for (let i = 0; i < 7; i++) {
        output[i] = randomColor(0.4, 0.86).rgbString();
      }
      return output;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">

@import "../assets/less/main.less";

#explain {
  margin: 0em 1.5em 1.5em 1.5em;
  max-width: 1600px;
}
.borderAddition {
  border: 1px solid @borderColor;
}
@media screen and (min-width: 1600px) {
  #explain {
    margin: 0em auto 2em auto;
  }
}
</style>
