<template>
  <div id="list">
    <div class="uk-card uk-card-default uk-card-body" id="tabsTitle">
      <ul class="uk-flex-left" uk-tab>
        <li class="uk-active">
          <h2 class="tabsTitleH2">Novel Database</h2>
        </li>
      </ul>
    </div>
    <div class="uk-card uk-card-default uk-card-body" id="tabsOuter">
      <ul class="uk-flex-left" uk-tab id="tabs">
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
          <a href="#" @click="tabsClick('book')">Traditional Books</a>
        </li>
        <li>
          <a href="#" @click="tabsClick('other')">Others</a>
        </li>
        <li>
          <a href="#" @click="tabsClickDropped()">Dropped</a>
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
          <h3>{{props.row.Title}}</h3>
        </span>
        <span v-if="props.column.field == 'Description'">
          <ul uk-accordion>
            <li>
              <a class="uk-accordion-title" href="#">
                <h3>Description</h3>
              </a>
              <div class="uk-accordion-content">
                <p>{{props.row.Description}}</p>
              </div>
            </li>
          </ul>
        </span>
        <span v-if="props.column.field == 'Dropped'">
          <span
            :class="props.row.Dropped ? labelDanger : labelSuccess"
            v-if="props.row.Dropped"
          >Dropped</span>
          <span class="uk-label tableLabel">{{props.row.Origin}}</span>
        </span>
        <span v-if="props.column.field == 'Origin'">
          <span class="uk-label tableLabel">{{props.row.Origin}}</span>
        </span>
        <span v-if="props.column.field == 'Picture'">
          <img rel="preconnect" :src="props.row.Picture" class="tablePicture" :alt="props.row.Title + ' Cover'"/>
        </span>
        <span v-if="props.column.field == 'Link'">
          <a rel="preconnect" :href="props.row.Link" class="uk-button uk-button-default">Link</a>
        </span>
        <span v-if="props.column.field == 'Rank'">
          <h3>{{props.row.Rank}}</h3>
        </span>
        <span v-if="props.column.field == 'Date'">
          <h3>{{props.row.Date}}</h3>
        </span>
      </template>
    </vue-good-table>
  </div>
</template>

<script>
// import the styles
import "../assets/less/table.css";
import { VueGoodTable } from "vue-good-table";
import variables from "../assets/data/variables.json";

export default {
  name: "NovelsList",
  props: { novels: Array },
  components: {
    VueGoodTable
  },
  watch: {
    // whenever novels changes, this function will run
    novels: function(newVal, oldVal) {
      this.tableNovels = newVal;
    }
  },
  data() {
    return {
      tableNovels: [],
      labelDanger: "uk-label uk-label-danger tableLabel",
      labelSuccess: "uk-label uk-label-success tableLabel",
      tableSearch: variables.tableSearch,
      tableSort: variables.tableSort,
      tablePagination: variables.tablePagination,
      columns: variables.columns
    };
  },
  methods: {
    log() {
      console.log("works");
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
#tabsTitle {
  margin: 0em 1.5em 0em 1.5em;
  padding: 10px 0px 0px 0px;
  border: 1px solid #4d4e5234;
  border-width: 1px 1px 0px 1px;
  display: flex;
  justify-content: center; 
}
.tabsTitleH2 {
  margin: 0;
}
#tabsOuter {
  margin: 0em 1.5em 0em 1.5em;
  padding: 10px 0px 0px 0px;
  border: 1px solid #4d4e5234;
  border-width: 1px 1px 0px 1px;
}
#tabs {
  margin-bottom: 0;
}
#tabs::before {
  border-bottom-width: 0;
}
#tabs > .uk-active > a {
  background-color: rgba(57, 0, 90, 0.6);
  color: white;
}
.uk-tab > * {
  margin-bottom: 10px;
}
.uk-tab > * > a {
  border: 0px solid transparent;
  font-size: 150%;
}
.tablePicture {
  height: 150px;
}
.tableLabel {
  font-size: 100%;
}
#table {
  margin: 0em 1.5em 1.5em 1.5em;
}
</style>
