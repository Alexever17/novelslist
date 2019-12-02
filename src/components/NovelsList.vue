<template>
<!-- table component container -->
  <div id="list">
    <!-- table title -->
    <div class="uk-card uk-card-default uk-card-body tabsBorder" id="listTitle">
      <ul class="uk-flex-left" uk-tab>
        <li class="uk-active">
          <h2 class="tabsTitleH2">Novel Database</h2>
        </li>
      </ul>
    </div>
    <!-- table navigation -->
    <div class="uk-card uk-card-default uk-card-body tabsBorder" id="listNav">
      <ul class="uk-flex-left" uk-tab id="tabNavUl">
        <!-- when clicked it changes the entry base -> prefilter based on ategory of the books  -->
        <li class="uk-active">
          <a href="#" @click="tabsClick('full')">Full List</a>
        </li>
        <li>
          <a href="#" @click="tabsClick('chinese')">Chinese Translations</a>
        </li>
        <li>
          <a href="#" @click="tabsClick('korean')">Korean Translations</a>
        </li>
        <li>
          <a href="#" @click="tabsClick('japanese')">Japanese Translations</a>
        </li>
        <li>
          <a href="#" @click="tabsClick('webnovel')">Webnovel Originals</a>
        </li>
        <li>
          <a href="#" @click="tabsClick('other')">Other Originals</a>
        </li>
        <li>
          <a href="#" @click="tabsClick('book')">Traditional Books</a>
        </li>
        <!-- special function to sort for dropped books -->
        <li>
          <a href="#" @click="tabsClickDropped()">Stopped Reading (Dropped)</a>
        </li>
      </ul>
    </div>
<!-- 2 version of the table are created because changing the component with css only was too complicated
     one for desktop, one for mobile
     this is the desktop one -->
     <!-- takes in the options from the variable data in the assets -->
    <vue-good-table
      id="table"
      :columns="columns"
      :rows="tableNovels"
      :search-options="tableSearch"
      :pagination-options="tablePagination"
      :sort-options="tableSort"
    >
    <!-- it has templates to transform the incoming entry data -->
    <!-- some columns are hidden but are kept if they are needed later -->
      <template slot="table-row" slot-scope="props">
        <!-- title visible -->
        <span v-if="props.column.field == 'Title'">
          <h4>{{ props.row.Title }}</h4>
        </span>
        <!-- description visible-->
        <span v-if="props.column.field == 'Description'">
          <!-- a preview is always generated on the desktop version -->
          <!-- depending on width the preview display the whole text or makes an accordion with the rest -->
          <!-- lengthchecker determines that based on an exponention function tied to width -->
          <div v-if="lengthChecker(props.row.Description)[0]">
            <h4>Description</h4>
            <p class="tableP fontColorChange">{{ props.row.Description }}</p>
          </div>
          <ul v-else uk-accordion>
            <li>
              <a class="uk-accordion-title" href="#">
                <h4>Description</h4>
                <!-- preview is the visible part from the start  -->
                <p class="tableP">{{ preview(props.row.Description) }}</p>
              </a>
              <div class="uk-accordion-content">
                <!-- afterPreview is the hidden part where you have to click the + -->
                <p>{{ afterPreview(props.row.Description) }}</p>
              </div>
            </li>
          </ul>
        </span>
        <!-- dropped label hidden -->
        <span v-if="props.column.field == 'Dropped'">
          <span
            :class="props.row.Dropped ? labelDanger : labelSuccess"
            v-if="props.row.Dropped"
            >Dropped</span
          >
          <span class="uk-label tableLabel">{{ props.row.Origin }}</span>
        </span>
        <!-- origin, hidden -->
        <span v-if="props.column.field == 'Origin'">
          <span class="uk-label tableLabel">{{ props.row.Origin }}</span>
        </span>
        <!-- picture visible -->
        <span v-if="props.column.field == 'Picture'">
          <img
            rel="preconnect"
            :src="props.row.Picture"
            class="tablePicture"
            :alt="props.row.Title + ' Cover'"
          />
        </span>
        <!-- opens up a modal with a button for further info, visible -->
        <span v-if="props.column.field == 'Link'">
          <Modal :novel="props.row" uk-modal />
          <a
            class="uk-button uk-button-default tableInfo"
            type="button"
            :uk-toggle="'target: #' + 'modal' + modalIdMaker(props.row.Title)"
            >More Info</a
          >
        </span>
        <!-- ranking visible -->
        <span v-if="props.column.field == 'Rank'">
          <h4>{{ props.row.Rank }} / 10</h4>
        </span>
        <!-- date when added, visible -->
        <span v-if="props.column.field == 'Date'">
          <h4>{{ props.row.Date }}</h4>
        </span>
      </template>
    </vue-good-table>

    <!-- mobile version of the table -->
    <!-- has its own variable in the variable.json -->
    <vue-good-table
      id="tableMobile"
      :columns="columnsMobile"
      :rows="tableNovels"
      :search-options="tableSearch"
      :pagination-options="tablePaginationMobile"
      :sort-options="tableSortMobile"
    >
    <!-- template to transfrom the incoming entry row -->
      <template slot="table-row" slot-scope="props">
        <!-- Information part with title, raniking and modalbutton and modal -->
        <span v-if="props.column.field == 'Title'">
          <h3>{{ props.row.Title }}</h3>
          <div class="uk-flex uk-flex-between uk-flex-middle mobileFlexChange">
            <div>
              <h4 class="noMargin">Ranking: {{ props.row.Rank }} / 10</h4>
            </div>
            <div>
              <Modal :novel="props.row" uk-modal />
              <!-- modalIdMaker makes the title to a functional ID -->
              <a
                class="uk-button uk-button-default tableInfo"
                type="button"
                :uk-toggle="
                  'target: #' + 'modal' + modalIdMaker(props.row.Title)
                "
                >More Info</a
              >
            </div>
          </div>
        </span>
        <!-- small preview picture -->
        <span v-if="props.column.field == 'Picture'">
          <img
            rel="preconnect"
            :src="props.row.Picture"
            class="tablePictureMobile"
            :alt="props.row.Title + ' Cover'"
          />
        </span>
      </template>
    </vue-good-table>
  </div>
</template>

<script>
// import the styles
import "../assets/less/component-stylesheets/table.css";
import { VueGoodTable } from "vue-good-table";
import Modal from "./Modal.vue";
// variable for the table to make component leaner
import variables from "../assets/data/variables.json";

export default {
  name: "NovelsList",
  props: { novels: Array },
  components: {
    VueGoodTable,
    Modal
  },
  watch: {
    // whenever novels changes, this function will run
    // necessary to detect api call
    novels: function(newVal, oldVal) {
      this.tableNovels = newVal;
    }
  },
  data() {
    return {
      //placeholder until load later novel entries
      tableNovels: variables.placeholderNovels,
      //for disabled column, may be used later
      //used for the dropped label to change colors
      labelDanger: "uk-label uk-label-danger tableLabel",
      labelSuccess: "uk-label uk-label-success tableLabel",
      //variables from variable.json, represent the columns and options for the table components
      tableSearch: variables.tableSearch,
      tableSort: variables.tableSort,
      tableSortMobile: variables.tableSortMobile,
      tablePagination: variables.tablePagination,
      tablePaginationMobile: variables.tablePaginationMobile,
      columns: variables.columns,
      columnsMobile: variables.columnsMobile
    };
  },
  methods: {
    // creates id out of novel title
    modalIdMaker(input) {
      return input.replace(/\W+/g, "");
    },
    //generates a small preview part of the description on desktop
    //it cuts after a sentence is over
    //is not used when description is smaller the lengthChecker output number
    preview(input) {
      //gives back array [false or true, number where to cut the description]
      let result = this.lengthChecker(input);

      //lengthchecker returns an array with boolean if the description is longer than the cut for the width and 
      //determined count
      // 
      if (result[0]) {
        return input;
      } else {
        let operation = input.indexOf(". ", result[1] - 10);
        if (operation == -1) {
          return input;
        } else {
          return input.slice(0, input.indexOf(". ", result[1] - 10) + 2);
        }
        
      }
    },
    // generates the overflow text after the preview
    //is not used when description is smaller the lengthChecker output number
    afterPreview(input) {
      //gives back array [false or true, number where to cut]
      let result = this.lengthChecker(input);

      if (result[0]) {
        return "---";
      } else {
        let operation = input.indexOf(". ", result[1] - 10);
        if (operation == -1) {
          return input;
        } else {
          return input.slice(input.indexOf(". ", result[1] - 10) + 2);
        }
      }
    },
    //based on an exponential function return the number of characters that should be shown in the preview.
    //returns an array with the boolean if description is longer than the determined char count and that count
    lengthChecker(input) {
      let width = Math.max(
        document.documentElement.clientWidth,
        window.innerWidth || 0
      );
      let factor = Math.abs((-8 * width) / 1000 + 16);
      let division = Math.round(width / factor);

      if (input.length < division) {
        return [true, 0];
      } else {
        return [false, division];
      }
    },
    //filters the entries (all novels) based on the category selected
    tabsClick(input) {
      //all novels
      if (input == "full") {
        this.tableNovels = this.novels;
        // counts all those not in the main catagory like royalroad
      } else if (input == "other") {
        this.tableNovels = this.novels.filter(
          el =>
            el.Origin != "chinese" &&
            el.Origin != "korean" &&
            el.Origin != "japanese" &&
            el.Origin != "webnovel" &&
            el.Origin != "book"
        );
        // filters with specific input from menu
      } else {
        this.tableNovels = this.novels.filter(el => el.Origin == input);
      }
    },
    //filters the entries (all novels) for dropped novels
    tabsClickDropped() {
      this.tableNovels = this.novels.filter(el => el.Dropped == true);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
//main webpage css
@import "../assets/less/main.less";
//specific component css to keep component leaner
@import "../assets/less/tableComponent.less";
</style>
