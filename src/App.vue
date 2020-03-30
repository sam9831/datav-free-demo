<template>
  <div id="app">
    <sales-report></sales-report>
  </div>
</template>

<script>
import SalesReport from './components/SalesReport'

export default {
  name: 'App',
  components: {
    SalesReport
  }
}
</script>

<style>
html, body {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
  background: #eee;
}
#app {
  width: 100%;
  height: 100%;
  padding: 20px;
  box-sizing: border-box;
}
</style>
