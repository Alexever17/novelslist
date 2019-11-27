<template>
  <div id="explainer" class="uk-flex uk-flex-center">
    <div
      class="uk-child-width-1-2@m uk-child-width-1-2@l uk-grid-medium uk-grid-match uk-flex-center uk-width-1-1"
      uk-grid
    >
      <div>
        <div class="uk-card uk-card-default uk-card-body border">
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

                <span class="longExplainText">
                  <br />Sometimes it is difficult to find the most interesting
                  novels, because many are translated by fans and aren't
                  released officially. There are multiple sources to find novels
                  but the two most used are novelupdates.com and webnovel.com.
                  Novelupdates is a website which tracks all translated novels
                  and has their information. There you can find the licensing
                  information and where you can read it. Webnovel meanwhile is
                  the biggest plattform for translated and original
                  novels.</span
                >
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
          <p>
            Click on a category to disable it from the chart or add it again.
          </p>
          <PieChart v-if="loaded" :options="options" :chartdata="chartdata" />
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
  watch: {
    // whenever novels changes, this function will run
    novels: function(newVal, oldVal) {
      var c = this.groupBy(newVal, "Origin");
      let dropped = this.groupBy(newVal, "Dropped");
      let sumDropped = dropped.true[0];
      let sumCategory = [
        c.chinese[0],
        c.japanese[0],
        c.korean[0],
        c.webnovel[0],
        c.book[0],
        c.other[0],
        sumDropped
      ];

      this.chartdata = {
        labels: this.chartlabels,
        datasets: [
          {
            label: "Alexever17 Novels",
            backgroundColor: this.colorCreator(),
            data: sumCategory
          }
        ]
      };
      this.loaded = true;
    }
  },
  data() {
    return {
      loaded: false,
      chartdata: null,
      chartlabels: [
        "Chinese Novels",
        "Korean Novels",
        "Japanese Novels",
        "Webnovel Originals",
        "Traditional Books",
        "Other Websites",
        "Dropped Novels"
      ],
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
    groupBy(objectArray, property) {
      return objectArray.reduce(function(acc, obj) {
        var key = obj[property];
        if (!acc[key]) {
          acc[key] = [];
        }
        if (acc[key][0]) {
          acc[key][0]++;
        } else {
          acc[key].push(1);
        }
        return acc;
      }, {});
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

#explainer {
  width: 100%;
  margin: 2em 2em 0em 2em;
  max-width: 1800px;
}

.border {
  border: 1px solid @borderColor;
}

//enables max-width for the explainer component, otherwise a padding form an inside component is interfearing
[class*="uk-width"] {
  width: unset;
  max-width: unset;
}

.longExplainText {
  .over1000();
}
</style>
