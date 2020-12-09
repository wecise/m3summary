<!-- 大号数字 单值 -->
<template>
  <div style="width: 120px; height: 100%; padding: 20px; text-align: center">
    <span style="color: rgb(153, 153, 153)">{{title}}</span>
    <h1>
      {{ value }}
      <span style="color: rgb(153, 153, 153); font-size: x-small">{{
        unit
      }}</span>
    </h1>
  </div>
</template>

<script>
export default {
  name: "LargerNumber",
  props: {
    title: String,
    value: {
      type: Number,
      default: 0
    },
    unit: String,
  },
  data() {
    return {
      valueTween: 0,
    }
  },
  watch: {
    value: {
      handler:function(newValue) {
        this.valueTween = newValue;
        //TweenLite.to(this.$data, 1, { valueTween: newValue });
      },
      immediate: true,
    },
  },
};
</script>
<style scoped>
h1{
  font-size: 32px;
}
</style>