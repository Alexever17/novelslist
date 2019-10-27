<template>
  <div>
    <vue-good-table id="list" :columns="columns" :rows="rows" />
  </div>
</template>

<script>
// import the styles
import "vue-good-table/dist/vue-good-table.css";
import { VueGoodTable } from "vue-good-table";

export default {
  name: "List",
  props: { novels: Array },
  components: {
    VueGoodTable
  },
  methods: {
    log() {
      console.log(this.$props.novels[0])
    }
  },
  data() {
    return {
      columns: [
        {
          label: "Title",
          field: "Title"
        },
        {
          label: "Created On",
          field: "Date",
          type: "date",
          dateInputFormat: "dd.MM.yyyy",
          dateOutputFormat: "dd.MM.yyyy"
        },
        {
          label: "Description",
          field: "Description",
          width: '400px'
        },
        {
          label: "Dropped",
          field: "Dropped",
          type: "boolean"
        },
        {
          label: "Link",
          field: "Link"
        },
        {
          label: "Origin",
          field: "Origin"
        },
        {
          label: "Picture",
          field: "Picture"
        },
        {
          label: "Progress",
          field: "Progress"
        },
        {
          label: "Rank",
          field: "Rank",
          type: "number"
        },
        
      ],
      rows: this.$props.novels
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#list {
  margin: 0em 1em 1em 1em;
}
</style>

