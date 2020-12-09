<!-- 大号数字 单值 多列样式 -->
<template>
  <div
    style="
      width: 120px;
      height: 100%;
      padding: 20px;
      text-align: center;
      background: #eff4f6;
    "
  >
    <span style="color: rgb(153, 153, 153)">{{title}}</span>
    <h1 style="font-size: 32px">
      {{valueTween}}
      <span style="color: rgb(153, 153, 153); font-size: x-small"
        >{{unit}}</span
      >
    </h1>
  </div>
</template>
    <script>
export default {
  name: "LargerColumnNumber.",
  delimiters: ["#{", "}#"],
  props: {
    title: String,
    value: Number,
    unit: String,
  },
  data() {
    return {
      valueTween: 0,
    };
  },
  watch: {
    value: {
      handler: function (newValue) {
        //TweenLite.to(this.$data, 1, { valueTween: newValue });
        this.valueTween = newValue;
      },
      immediate: true,
    },
  },
};
</script>