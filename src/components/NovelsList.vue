<template>
  <div id="list">
    <div class="uk-card uk-card-default uk-card-body tabsBorder" id="listTitle">
      <ul class="uk-flex-left" uk-tab>
        <li class="uk-active">
          <h2 class="tabsTitleH2">Novel Database</h2>
        </li>
      </ul>
    </div>
    <div class="uk-card uk-card-default uk-card-body tabsBorder" id="listNav">
      <ul class="uk-flex-left" uk-tab id="tabNavUl">
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
        <li>
          <a href="#" @click="tabsClickDropped()">Stopped Reading (Dropped)</a>
        </li>
      </ul>
    </div>

    <vue-good-table
      id="table"
      :columns="columns"
      :rows="tableNovels"
      :search-options="tableSearch"
      :pagination-options="tablePagination"
      :sort-options="tableSort"
    >
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field == 'Title'">
          <h4>{{ props.row.Title }}</h4>
        </span>
        <span v-if="props.column.field == 'Description'">
          <div v-if="lengthChecker(props.row.Description)[0]">
            <h4>Description</h4>
            <p class="tableP fontColorChange">{{ props.row.Description }}</p>
          </div>
          <ul v-else uk-accordion>
            <li>
              <a class="uk-accordion-title" href="#">
                <h4>Description</h4>
                <p class="tableP">{{ preview(props.row.Description) }}</p>
              </a>
              <div class="uk-accordion-content">
                <p>{{ afterPreview(props.row.Description) }}</p>
              </div>
            </li>
          </ul>
        </span>
        <span v-if="props.column.field == 'Dropped'">
          <span
            :class="props.row.Dropped ? labelDanger : labelSuccess"
            v-if="props.row.Dropped"
            >Dropped</span
          >
          <span class="uk-label tableLabel">{{ props.row.Origin }}</span>
        </span>
        <span v-if="props.column.field == 'Origin'">
          <span class="uk-label tableLabel">{{ props.row.Origin }}</span>
        </span>
        <span v-if="props.column.field == 'Picture'">
          <img
            rel="preconnect"
            :src="props.row.Picture"
            class="tablePicture"
            :alt="props.row.Title + ' Cover'"
          />
        </span>
        <span v-if="props.column.field == 'Link'">
          <Modal :novel="props.row" uk-modal />
          <a
            class="uk-button uk-button-default tableInfo"
            type="button"
            :uk-toggle="'target: #' + 'modal' + modalIdMaker(props.row.Title)"
            >More Info</a
          >
        </span>
        <span v-if="props.column.field == 'Rank'">
          <h4>{{ props.row.Rank }} / 10</h4>
        </span>
        <span v-if="props.column.field == 'Date'">
          <h4>{{ props.row.Date }}</h4>
        </span>
      </template>
    </vue-good-table>

    <vue-good-table
      id="tableMobile"
      :columns="columnsMobile"
      :rows="tableNovels"
      :search-options="tableSearch"
      :pagination-options="tablePaginationMobile"
      :sort-options="tableSortMobile"
    >
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field == 'Title'">
          <h3>{{ props.row.Title }}</h3>
          <div class="uk-flex uk-flex-between uk-flex-middle mobileFlexChange">
            <div>
              <h4 class="noMargin">Ranking: {{ props.row.Rank }} / 10</h4>
            </div>
            <div>
              <Modal :novel="props.row" uk-modal />
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
      for (let i = 0; i < newVal.length; i++) {
        this.disableData[newVal[i].Title] = true;
      }
      this.tableNovels = newVal;
    }
  },
  data() {
    return {
      tableNovels: variables.placeholderNovels,
      labelDanger: "uk-label uk-label-danger tableLabel",
      labelSuccess: "uk-label uk-label-success tableLabel",
      disableData: {},
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
    modalIdMaker(input) {
      return input.replace(/\W+/g, "");
    },
    preview(input) {
      //gives back array [false or true, number where to cut]
      let result = this.lengthChecker(input);

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
    tabsClick(input) {
      if (input == "full") {
        this.tableNovels = this.novels;
      } else if (input == "other") {
        this.tableNovels = this.novels.filter(
          el =>
            el.Origin != "chinese" &&
            el.Origin != "korean" &&
            el.Origin != "japanese" &&
            el.Origin != "webnovel" &&
            el.Origin != "book"
        );
      } else {
        this.tableNovels = this.novels.filter(el => el.Origin == input);
      }
    },
    tabsClickDropped() {
      this.tableNovels = this.novels.filter(el => el.Dropped == true);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@import "../assets/less/main.less";
@import "../assets/less/tableComponent.less";
</style>
