<template>
  <div id="app">
    <WebTest msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import WebTest from './components/WebTest.vue'

export default {
  name: 'app',
  components: {
    WebTest
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
