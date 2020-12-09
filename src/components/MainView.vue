<template>
  <el-container>
    <el-header style="height: 40px; line-height: 40px; padding: 0px">
      <h5 style="margin: 0px; float: left">{{ title }}</h5>
    </el-header>
    <el-main style="padding: 0px; overflow: hidden" v-if="model">
      <el-row :gutter="20">
        <el-col
          :span="24"
          style="
            padding: 20px;
            box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
            background-color: #ffffff;
          "
        >
          <h5 style="margin: 10px; float: left">产品</h5>
          <div style="display: flex; float: left; flex-wrap: wrap">
            <div
              @click="onForward(item.url)"
              :key="item.name"
              v-for="item in model.m3"
              style="
                cursor: pointer;
                width: 32.3%;
                min-width: 320px;
                height: 8em;
                margin: 0 0.5% 30px;
                background: #eff4f6;
                display: flex;
                float: left;
              "
            >
              <span style="padding: 30px 20px">
                <el-image
                  :src="item.icon | pickIcon"
                  style="
                    width: 34px;
                    filter: grayscale(100%) brightness(45%) sepia(100%)
                      hue-rotate(-180deg) saturate(700%) contrast(0.8);
                  "
                ></el-image>
              </span>
              <div style="text-align: left; padding: 0 10px">
                <h4>
                  <span v-if="MATRIX_LANG == 'zh-CN'">{{ item.cnname }}</span>
                  <span v-else>{{ item.enname }}</span>
                </h4>
                <p style="font-size: 10px; color: #888888">{{ item.desc }}</p>
              </div>
            </div>
          </div>
        </el-col>

        <el-col
          :span="24"
          style="
            margin-top: 20px;
            padding: 20px;
            box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
            background-color: #ffffff;
          "
        >
          <h5 style="margin: 10px; float: left">已发布小应用</h5>
          <div style="display: flex; float: left; flex-wrap: wrap">
            <div
              @click="onForward(item.url)"
              :key="item.name"
              v-for="item in model.app"
              style="
                cursor: pointer;
                width: 32.3%;
                min-width: 320px;
                height: 8em;
                margin: 0 0.5% 30px;
                background: #eff4f6;
                display: flex;
                float: left;
              "
            >
              <span style="padding: 30px 20px">
                <el-image
                  :src="item.icon | pickIcon"
                  style="
                    width: 34px;
                    filter: grayscale(100%) brightness(45%) sepia(100%)
                      hue-rotate(-180deg) saturate(700%) contrast(0.8);
                  "
                ></el-image>
              </span>
              <div style="text-align: left; padding: 0 10px">
                <h4>
                  <span v-if="MATRIX_LANG == 'zh-CN'">{{ item.cnname }}</span>
                  <span v-else>{{ item.enname }}</span>
                </h4>
                <p style="font-size: 10px; color: #888888">{{ item.desc }}</p>
              </div>
            </div>
          </div>
        </el-col>
        <el-col
          :span="24"
          style="
            margin-top: 20px;
            padding: 20px;
            box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
            background-color: #ffffff;
          "
        >
          <h5 style="margin: 10px; text-align: left">接口服务概览</h5>
          <div style="display: flex; float: left">
            <div
              @click="onForward(item.url)"
              :key="item.name"
              v-for="item in model.api"
              :style="
                'cursor: pointer;width: 32.3%;min-width: 320px;height:8em;margin: 0 .5% 30px;display: flex;float: left;background: ' +
                item.background +
                ';color:#ffffff;'
              "
            >
              <span style="padding: 10px">
                <el-image :src="item.icon | pickIcon"></el-image>
              </span>
              <div style="text-align: left; padding: 0 10px">
                <h4>
                  <span v-if="MATRIX_LANG == 'zh-CN'">{{ item.cnname }}</span>
                  <span v-else>{{ item.enname }}</span>
                </h4>
                <p style="font-size: 34px; margin: 0px; font-weight: 900">
                  {{ item.count }}
                </p>
              </div>
            </div>
          </div>
        </el-col>

        <el-col
          :span="24"
          style="
            margin-top: 20px;
            padding: 20px;
            box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
            background-color: #ffffff;
          "
          v-if="dataModel"
        >
          <h5 style="margin: 10px; float: left">数据服务概览(当日统计)</h5>

          <div
            style="
              display: flex;
              float: left;
              background: #eff4f6;
              width: 99%;
              margin: 0px 10px;
            "
          >
            <LargerNumber
              title="数据吞吐量"
              :value="dataModel.byDay.throughput"
              unit="条/秒"
            >
            </LargerNumber>

            <LargerNumber
              title="数据响应时间(avg)"
              :value="dataModel.byDay.span_avg"
              unit="毫秒"
            >
            </LargerNumber>

            <LargerNumber
              title="数据响应时间(max)"
              :value="dataModel.byDay.span_max"
              unit="毫秒"
            >
            </LargerNumber>

            <LargerNumber
              title="数据响应时间(min)"
              :value="dataModel.byDay.span_min"
              unit="毫秒"
            >
            </LargerNumber>

            <LargerNumber
              title="数据总量(当日)"
              :value="dataModel.byDay.all_volume | pickValue"
              :unit="dataModel.byDay.all_volume | pickUnit"
            >
            </LargerNumber>

            <LargerNumber
              title="磁盘利用率"
              :value="dataModel.byDay.disk_volume_percent | pickValue"
              unit="%"
            >
            </LargerNumber>
          </div>
          <div
            style="
              display: flex;
              float: left;
              background: #eff4f6;
              width: 99%;
              margin: 0px 10px;
            "
          >
            <pie-chart
              title="当日"
              :model="dataModel.byApi"
              v-if="dataModel.byApi"
            ></pie-chart>

            <CurveChartByDay
              title="当日趋势"
              :model="dataModel.byDay"
              v-if="dataModel.byDay"
            ></CurveChartByDay>
          </div>
        </el-col>

        <el-col
          :span="24"
          style="
            margin-top: 20px;
            padding: 20px;
            box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
            background-color: #ffffff;
          "
          v-if="dataModel"
        >
          <h5 style="margin: 10px; float: left">数据服务概览(按接口统计)</h5>

          <div
            :key="api"
            v-for="(item, api) in dataModel.byApi"
            style="
              float: left;
              background: rgb(239, 244, 246);
              width: 99%;
              margin: 0px 10px;
              text-align:left;
            "
          >
            <h5 style="margin: 0px; padding: 10px 20px">
              {{ api }}
            </h5>
            <div
              style="
                display: flex;
                float: left;
                background: #eff4f6;
                width: 99%;
              "
            >
              <LargerColumnNumber
                :title="item.volume.title"
                :value="item.volume.value"
                unit="笔/秒"
              >
              </LargerColumnNumber>

              <LargerColumnNumber
                :title="kpi.title | pickTitle"
                :value="kpi.value"
                unit="毫秒"
                :key="kpi.title"
                v-for="kpi in item.summary"
              >
                ></LargerColumnNumber
              >

              <CurveChartByApi
                title="当日趋势"
                height="180px"
                :model="dataModel.byApi[api]"
                ref="curveChart"
              ></CurveChartByApi>
            </div>
          </div>
        </el-col>
      </el-row>
    </el-main>
    <el-footer> </el-footer>
  </el-container>
</template>

<script>
const axios = require("axios").default;

import LargerNumber from "./LargerNumber.vue";
import LargerColumnNumber from "./LargerColumnNumber.vue";
import PieChart from "./PieChart.vue";
import CurveChartByApi from "./CurveChartByApi.vue";
import CurveChartByDay from "./CurveChartByDay.vue";

export default {
  name: "MainView",
  props: {
    title: String,
  },
  components: {
    LargerNumber,
    LargerColumnNumber,
    PieChart,
    CurveChartByApi,
    CurveChartByDay,
  },
  data() {
    return {
      model: null,
      dataModel: null,
      MATRIX_LANG: window.MATRIX_LANG,
    };
  },
  filters: {
    pickValue(val) {
      let bytesToSize = (bytes) => {
        var sizes = ["Bytes", "KB", "MB", "GB", "TB"];
        if (bytes == 0) return "0 Byte";
        var i = parseInt(Math.floor(Math.log(bytes) / Math.log(1024)));
        return Math.round(bytes / Math.pow(1024, i), 2) + " " + sizes[i];
      };
      return Number(bytesToSize(val).split(" ")[0]);
    },
    pickUnit(val) {
      let bytesToSize = (bytes) => {
        var sizes = ["Bytes", "KB", "MB", "GB", "TB"];
        if (bytes == 0) return "0 Byte";
        var i = parseInt(Math.floor(Math.log(bytes) / Math.log(1024)));
        return Math.round(bytes / Math.pow(1024, i), 2) + " " + sizes[i];
      };
      return bytesToSize(val).split(" ")[1];
    },
    pickTitle(title) {
      return `${title}`;
    },
    pickIcon(icon) {
      return `${window.base}${icon}?sessionid=${window.session}`;
    },
  },
  created() {
    this.initSession();
  },
  mounted() {
    setInterval(() => {
      this.initSummary();
    }, 30 * 1000);
  },
  methods: {
    // 单位转换
    bytesToSize(bytes) {
      var sizes = ["Bytes", "KB", "MB", "GB", "TB"];
      if (bytes == 0) return "0 Byte";
      var i = parseInt(Math.floor(Math.log(bytes) / Math.log(1024)));
      return Math.round(bytes / Math.pow(1024, i), 2) + " " + sizes[i];
    },
    initSession() {
      axios
        .get(`${window.base}/user/signin?${window.signin}`)
        .then((rtn) => {
          window.session = rtn.data.message;
          //this.initApps();
          this.initSummary();
        })
        .catch((error) => {
          console.log(error);
        })
        .then(() => {});
    },
    initApps() {
      axios
        .post(
          `${window.base}/script/exec/js?input=null&isfile=true&filepath=/matrix/user/user.js&sessionid=${window.session}`
        )
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error);
        })
        .then(() => {});
    },
    onForward(url) {
      window.open(url, "_parent");
    },
    initSummary() {
      axios
        .post(
          `${window.base}/script/exec/js?input=null&isfile=true&filepath=/matrix/summary/getSummary.js&sessionid=${window.session}`
        )
        .then((rtn) => {
          this.model = rtn.data.message;
          this.initDataSummary();
        })
        .catch((error) => {
          console.log(error);
        })
        .then(() => {});
    },
    initDataSummary() {
      axios
        .post(
          `${window.base}/script/exec/js?input=null&isfile=true&filepath=/matrix/summary/getDataSummary.js&sessionid=${window.session}`
        )
        .then((rtn) => {
          this.dataModel = rtn.data.message;
        })
        .catch((error) => {
          console.log(error);
        })
        .then(() => {});
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
