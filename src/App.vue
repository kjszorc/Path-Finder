<template>
  <div id="app">
    <img src="./assets/logo.png">
    <PathFinder/>
  </div>
</template>

<script>

import PathFinder from './components/PathFinder'

export default {
  name: 'App',
  components: {
    PathFinder
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
  body {
    background-color: rgb(6, 46, 46);
  }
</style>
