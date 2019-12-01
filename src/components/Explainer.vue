<template>
  <div id="explainer" class="uk-flex uk-flex-center">
    <div
      class="uk-child-width-1-2@m uk-child-width-1-2@l uk-grid-medium uk-grid-match uk-flex-center uk-width-1-1"
      uk-grid
    >
      <div>
        <div class="uk-card uk-card-default uk-card-body border">
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
                >
              </div>
              <!-- the message itself -->
              <div class="uk-width-expand">
                <h4 class="uk-comment-title uk-margin-remove">
                  <a class="uk-link-reset" href="#">Alex</a>
                </h4>
                <ul class="uk-comment-meta uk-subnav uk-subnav-divider uk-margin-remove-top">
                  <li>
                    <span>Hobby web dev, reader of novels</span>
                  </li>
                </ul>
              </div>
            </header>
            <div class="uk-comment-body">
              <h3 class="uk-card-title">Welcome!</h3>
              <p>
                I love reading web novels and want to share my passion with you.
                On this website you can find all novels I ever read.
                <span
                  class="longExplainText"
                >
                  <br>Sometimes it is difficult to find the most interesting
                  novels, because many are translated by fans and aren't
                  released officially. There are multiple sources to find novels
                  but the two most used are novelupdates.com and webnovel.com.
                  Novelupdates is a website which tracks all translated novels
                  and has their information. There you can find the licensing
                  information and where you can read it. Webnovel meanwhile is
                  the biggest plattform for translated and original
                  novels.
                </span>
              </p>
              <a href="https://novelupdates.com">Novel Updates</a>
              <br>
              <a href="https://webnovel.com">Webnovel</a>
            </div>
          </article>
        </div>
      </div>
      <div>
        <div class="uk-card uk-card-default uk-card-body border">
          <h3 class="uk-card-title">Database Information</h3>
          <p>Click on a category to disable it from the chart or add it again.</p>
          <PieChart v-if="loaded" :options="options" :chartdata="chartdata"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PieChart from "./PieChart.vue";
import randomColor from "random-color";
import { normal } from "color-blend";

export default {
  name: "Explainer",
  props: { novels: Array },
  watch: {
    // whenever novels changes, this function will run
    novels: function(newVal, oldVal) {
      let sumCategory = this.groupingNovels(newVal);

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
    groupingNovels(input) {
      var c = this.groupBy(input, "Origin");
      let otherEntries = this.nonMainCategoriesCounter(c);

      let dropped = this.groupBy(input, "Dropped");
      let sumDropped = dropped.true[0];

      return [
        c.chinese[0],
        c.japanese[0],
        c.korean[0],
        c.webnovel[0],
        c.book[0],
        otherEntries,
        sumDropped
      ];
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
    nonMainCategoriesCounter(c) {
      let result = 0;
      let cKeys = Object.keys(c);
      let otherKeys = [];

      for (let i = 0; i < cKeys.length; i++) {
        if (
          cKeys[i] != "chinese" &&
          cKeys[i] != "korean" &&
          cKeys[i] != "japanese" &&
          cKeys[i] != "webnovel" &&
          cKeys[i] != "book"
        ) {
          otherKeys.push(cKeys[i]);
        }
      }

      for (let y = 0; y < otherKeys.length; y++) {
        result += c[otherKeys[y]][0];
      }

      return result;
    },
    colorCreator() {
      let output = [];
      let themeColor = { r: 0, g: 0, b: 255, a: 0.35 };
      for (let i = 0; i < 7; i++) {
        let random = randomColor(0.4, 0.86).rgbaString();
        let generatedColor = { r: random.slice(5,8), g: random.slice(10,13), b: random.slice(16,18), a: 0.7 };
        let mixed = normal(generatedColor, themeColor)
        output[i] = `rgba(${mixed.r}, ${mixed.g}, ${mixed.b}, .65)`;
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

@media only screen and (max-width: 480px) {
  #explainer {
    margin: 2em 0em 0em 0em;
  }
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
