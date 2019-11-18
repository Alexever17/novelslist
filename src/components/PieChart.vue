<template>
  <svg width="330" height="220">
    <g style="transform: translate(15px, 10px)">
      <path :d="line" />
    </g>
  </svg>
</template>

<script>
import * as d3 from "d3";

export default {
  name: "PieChart",
  props: { novel: Object },
  data() {
    return {
      data: [63, 15, 24, 17, 1, 2, 58],
      line: ""
    };
  },
  mounted() {
    this.calculatePath();
  },
  methods: {
    getScales() {
      const x = d3.scaleTime().range([0, 300]);
      const y = d3.scaleLinear().range([200, 0]);
      d3.axisLeft().scale(x);
      d3.axisBottom().scale(y);
      x.domain(d3.extent(this.data, (d, i) => i));
      y.domain([0, d3.max(this.data, d => d)]);
      return { x, y };
    },
    calculatePath() {
      const scale = this.getScales();
      const path = d3
        .line()
        .x((d, i) => scale.x(i))
        .y(d => scale.y(d));
      this.line = path(this.data);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@import "../assets/less/main.less";

svg {
  background-color: @mainColorLight;
}

path {
  fill: none;
  stroke: #76bf8a;
  stroke-width: 3px;
}
</style>
