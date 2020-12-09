<template>
  <div id="app">
    <MainView :title="title"/>
  </div>
</template>

<script>
import MainView from './components/MainView.vue'

export default {
  name: 'app',
  components: {
    MainView
  },
  data(){
    return {
      title:""
    }
  },
  created(){
    this.title = `今天是 ${this.moment().format("LLLL")}，欢迎回来。`;
  }
}
</script>

<style>
body{
  background: #f2f2f2;
  font-size: 12px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
