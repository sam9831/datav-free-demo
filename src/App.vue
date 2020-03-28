<template>
  <div id="app">
    <sales-report />
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
    margin: 0;
    padding: 0;
  }

  #app {
    width: 100%;
    height: 100%;
  }
</style>
