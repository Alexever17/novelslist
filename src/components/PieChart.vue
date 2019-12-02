<script>
//Doughnut shaped chart
import { Doughnut } from 'vue-chartjs'

export default {
  extends: Doughnut,
  name: "PieChart",
  //uses 2 inputs, the data and the options variables
  props: {
    chartdata: {
      type: Object,
      default: null
    },
    options: {
      type: Object,
      default: null
    }
  },
  mounted () {
    //wihtout it the chart does not display
    this.renderChart(this.chartdata, this.options)
  }
};
</script>