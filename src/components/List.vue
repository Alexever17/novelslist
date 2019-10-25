<template>
  <div id="list">
    <p>TEST</p>
    <p>{{novels}}</p>
  </div>
</template>

<script>
export default {
  name: "List",
  props: {},
  methods: {},
  created() {
        this.$http.get(process.env.VUE_APP_databaseURL + 'posts.json').then(function(data){
            return data.json()
        }).then(function(data){
            var array = [];
            for (let key in data){
                data[key].id = key;
                array.push(data[key]);
            }
            this.novels = array;
            //console.log(this.blogs);
        });
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
#list {
  margin: 0em 1em 1em 1em;
}
</style>
