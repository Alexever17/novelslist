<template>
<!-- container for the 2 cards with infos and chart -->
  <div id="explainer" class="uk-flex uk-flex-center">
    <div
      class="uk-child-width-1-2@m uk-child-width-1-2@l uk-grid-medium uk-grid-match uk-flex-center uk-width-1-1"
      uk-grid
    >
      <div>
        <!-- cards ahve additional borders, standard card from uikit -->
        <div class="uk-card uk-card-default uk-card-body border">
          <article class="uk-comment">
            <header class="uk-comment-header uk-grid-medium uk-flex-middle uk-grid" uk-grid>
              <div class="uk-width-auto uk-first-column">
                <!-- admins profile pic -->
                <img
                  class="uk-comment-avatar"
                  src="https://cdn.proxer.me/avatar/274858_1Whw0i.png"
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
                    <span>Reader of novels</span>
                  </li>
                </ul>
              </div>
            </header>
            <div class="uk-comment-body">
              <h3 class="uk-card-title">Welcome!</h3>
              <p>
                I love reading web novels and want to share my passion with you.
                On this website you can find all novels I ever read.</p>
            </div>
          </article>
        </div>
      </div>
      <div>
        <div class="uk-card uk-card-default uk-card-body border">
          <h3 class="uk-card-title">Database Information</h3>
          <p>Click on a category to disable it from the chart or add it again.</p>
          <!-- chart component -->
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
    // necessary to detect api call
    novels: function(newVal, oldVal) {
      //functions which creates the array of numbers of database categories
      let sumCategory = this.groupingNovels(newVal);

      this.chartdata = {
        labels: this.chartlabels,
        datasets: [
          {
            label: "Alexever17 Novels",
            // creates an array of colors
            backgroundColor: this.colorCreator(),
            //earlier created array of bumbers
            data: sumCategory
          }
        ]
      };
      //chart is hidden until the data is generated because chart has bad update compatibility
      this.loaded = true;
    }
  },
  data() {
    return {
      //needs data before display --> api call
      loaded: false,
      chartdata: null,
      chartlabels: [
        "Chinese Novels",
        "Korean Novels",
        "Japanese Novels",
        "Webnovel Originals",
        "Traditional Books",
        "Other Websites"
      ],
      options: {
        responsive: true,
        maintainAspectRatio: false,
        //makes it half a doughnut
        circumference: Math.PI,
        rotation: -Math.PI
      }
    };
  },
  components: { PieChart },
  methods: {
    //creates the array of numbers to the categories of the entries of the database
    //input is the api call
    groupingNovels(input) {
      // groups by the label characteristic Origin
      var c = this.groupBy(input, "Origin");
      // extra variable to count categories which are not main ones together
      let otherEntries = this.nonMainCategoriesCounter(c);

      //returns the array
      return [
        c.chinese[0],
        c.japanese[0],
        c.korean[0],
        c.webnovel[0],
        c.book[0],
        otherEntries
      ];
    },
    //groups novels database by the origin label and returns just the categories with the numbers
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
    //excludes the main categories and counts together the rest so that misspelled labels will be accounted for
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
    //creates random colors for the chart which are blended in with blue to be more themed to the website
    colorCreator() {
      //let output = [];
      //pure blue on alpha of .35
      //let blueColor = { r: 0, g: 0, b: 255, a: 0.35 };
      //for (let i = 0; i < 7; i++) {
        //generates random colors with some specific variables
        //let random = randomColor(0.4, 0.86).rgbaString();
        // formats the colors for the blender
        //let generatedColor = { r: random.slice(5,8), g: random.slice(10,13), b: random.slice(16,18), a: 0.7 };
        //blending the colors
        //let mixed = normal(generatedColor, blueColor)
        //formating for display
        //output[i] = `rgba(${mixed.r}, ${mixed.g}, ${mixed.b}, .65)`;
      //}      
      return [
      `rgba(116, 0, 184, 0.65)`,
      `rgba(94, 96, 206, 0.65)`,
      `rgba(78, 168, 222, 0.65)`,
      `rgba(72, 191, 227, 0.65)`,
      `rgba(100, 223, 223, 0.65)`,
      `rgba(128, 255, 219, 0.65)`
    ];
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@import "../assets/less/main.less";

//container for the 2 cards
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

//hides the long text on mobile devices by using a function from the main.less
.longExplainText {
  .over1000();
}
</style>
