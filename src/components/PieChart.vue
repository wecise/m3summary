<!-- 饼图 -->
<template>
  <div style="width:40%;">
    <h5>{{title}}</h5>
  <v-chart
    :options="options"
    :autoresize="true"
  ></v-chart>
  </div>
</template>
<script>
import _ from "lodash"
import ECharts from 'vue-echarts'
import "echarts/lib/chart/pie"
import 'echarts/lib/component/tooltip'

export default {
  name: "PieChart",
  components: {
    'v-chart': ECharts
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
        color: [],
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)",
        },
        
        legend: {
          data: ['line'],
           type: 'scroll',
        orient: 'vertical',
        right: 10,
        top: 20,
        bottom: 20,
        },
        
        series: [
          {
            type: "pie",
            clockwise: false, //饼图的扇区是否是顺时针排布
            minAngle: 2, //最小的扇区角度（0 ~ 360）
            radius: ["50%", "70%"],
            center: ["30%", "50%"],
            avoidLabelOverlap: false,

            label: {
              normal: {
                show: false,
                position: "center",
                formatter: "{text|{b}}\n{c} ({d}%)",
                rich: {
                  text: {
                    color: "#666",
                    fontSize: 14,
                    align: "center",
                    verticalAlign: "middle",
                    padding: 8,
                  },
                  value: {
                    color: "#8693F3",
                    fontSize: 14,
                    align: "bottom",
                    verticalAlign: "middle",
                  },
                },
              },
              emphasis: {
                show: true,
                textStyle: {
                  fontSize: 24,
                },
              },
            },
            data: [],
          },
        ],
      },
      colors: [
        "#8d7fec",
        "#5085f2",
        "#e75fc3",
        "#f87be2",
        "#f2719a",
        "#fca4bb",
        "#f59a8f",
        "#fdb301",
        "#57e7ec",
        "#cf9ef1",
      ],
    };
  },
  created() {
    _.extend(this.options, {
      color: this.colors,
    });
    // _.extend(this.options.legend, { data: _.keys(this.model) });
    // console.log(_.keys(this.model),this.options.legend)
    _.extend(this.options.series[0], {
      data: _.map(this.model, (v, k) => {
        return { name: k, value: v.volume.value };
      }),
    });

    this.options.title.text = this.title;
  }
};
</script>
<style scoped>
.echarts {
  width: 100%;
  height: 300px;
  margin: 0 20%;
}
</style>