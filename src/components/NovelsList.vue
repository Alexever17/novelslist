<template>
  <div>
    <vue-good-table id="list" :columns="columns" :rows="novels" :search-options="tableSearch" :pagination-options="tablePagination">
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
          <span :class="props.row.Dropped ? labelDanger : labelSuccess">{{props.row.Dropped}}</span>
        </span>
        <span v-if="props.column.field == 'Origin'">
          <span class="uk-label tableLabel">{{props.row.Origin}}</span>
        </span>
        <span v-if="props.column.field == 'Picture'">
          <img :src="props.row.Picture" class="tablePicture" />
        </span>
        <span v-if="props.column.field == 'Link'">
          <a :href="props.row.Link" class="uk-button uk-button-default">Link</a>
        </span>
        <span v-if="props.column.field == 'Rank'">
          <h3>{{props.row.Rank}}</h3>
        </span>
      </template>
    </vue-good-table>
  </div>
</template>

<script>
// import the styles
import "vue-good-table/dist/vue-good-table.css";
import { VueGoodTable } from "vue-good-table";

export default {
  name: "NovelsList",
  props: { novels: Array, columns: Array },
  components: {
    VueGoodTable
  },
  data() {
    return {
      labelDanger: "uk-label uk-label-danger tableLabel",
      labelSuccess: "uk-label uk-label-success tableLabel",
      tableSearch: {
        enabled: true,
        trigger: "enter",
        skipDiacritics: true,
        placeholder: "Search this table"
      },
      tablePagination: {
        enabled: true,
        mode: "records",
        perPage: 5,
        position: "top",
        perPageDropdown: [3, 7, 9],
        dropdownAllowAll: false,
        setCurrentPage: 2,
        nextLabel: "next",
        prevLabel: "prev",
        rowsPerPageLabel: "Rows per page",
        ofLabel: "of",
        pageLabel: "page", // for 'pages' mode
        allLabel: "All"
      }
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#list {
  margin: 0em 1.5em 1.5em 1.5em;
}
.tablePicture {
  height: 150px;
}
.tableLabel {
  font-size: 100%;
}
</style>

