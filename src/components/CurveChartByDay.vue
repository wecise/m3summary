<!-- 曲线 byday -->
<template>
  <div style="width:60%;">
    <h5>{{title}}</h5>
  <v-chart :options="options" :autoresize="true"></v-chart>
  </div>
</template>
<script>
import _ from "lodash";
import ECharts from "vue-echarts";
import 'echarts/lib/chart/bar'
import 'echarts/lib/chart/line'
import 'echarts/lib/component/tooltip'

export default {
  name: "CurveChartByDay",
  components: {
    "v-chart": ECharts,
  },
  props: {
    model: Object,
    title: String,
  },
  data() {
    return {
      options: {
        title: {
          text: "",
          left: "center",
          textStyle: {
            fontSize: "14px",
            color: "#999",
          },
        },
        tooltip: {
          trigger: "axis",
        },
        legend: {
          color: [],
          data: [],
          left: "center",
          bottom: "bottom",
        },
        grid: {
          top: "middle",
          left: "3%",
          right: "4%",
          bottom: "3%",
          height: "80%",
          containLabel: true,
        },
        xAxis: {
          type: "category",
          data: [],
          axisLine: {
            lineStyle: {
              color: "#999",
            },
          },
        },
        yAxis: {
          type: "value",

          splitLine: {
            lineStyle: {
              type: "dashed",
              color: "#DDD",
            },
          },
          axisLine: {
            show: false,
            lineStyle: {
              color: "#333",
            },
          },
          nameTextStyle: {
            color: "#999",
          },
          splitArea: {
            show: false,
          },
        },
        series: [],
      },
      colors: ["#F58080", "#47D8BE", "#F9A589", "#FF0000"],
      kpi: ["span_avg", "span_max", "span_min", "throughput"],
    };
  },
  watch: {
    model: {
      handler(val) {
        this.addData(val);
      },
      deep: true,
    },
  },
  created() {
    this.initData();
    this.options.title.text = this.title;
  },
  mounted() {},
  methods: {
    initData() {
      // legend
      this.options.legend.color = this.colors;
      this.options.legend.data = this.kpi;
      // xAxis
      this.options.xAxis.data.push(this.moment().format("HH:mm:ss"));
      // series
      this.options.series = _.map(this.kpi, (v, index) => {
        return {
          name: v,
          type: "line",
          data: [this.model[v]],
          color: this.colors[index],
          top: "10%",
          bottomo: "30%",
          lineStyle: {
            normal: {
              width: 0.5,
              color: {
                type: "linear",

                colorStops: [
                  {
                    offset: 0,
                    color: "#FFCAD4", // 0% 处的颜色
                  },
                  {
                    offset: 0.4,
                    color: "#F58080", // 100% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "#F58080", // 100% 处的颜色
                  },
                ],
                globalCoord: false, // 缺省为 false
              },
              shadowColor: "rgba(245,128,128, 0.5)",
              shadowBlur: 10,
              shadowOffsetY: 7,
            },
          },
          itemStyle: {
            normal: {
              color: this.colors[index],
              borderWidth: 1,
              /*shadowColor: 'rgba(72,216,191, 0.3)',
                                                    shadowBlur: 100,*/
              borderColor: this.colors[index],
            },
          },
          smooth: true,
        };
      });
    },
    addData(val) {
      // xAxis
      this.options.xAxis.data.push(this.moment().format("HH:mm:ss"));
      // series
      _.forEach(this.kpi, (v, index) => {
        this.options.series[index].data.push(val[v]);
      });
    },
  },
};
</script>
<style>
.echarts {
  width: 100%;
  height: 300px;
}
</style>
