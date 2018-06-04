<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>

<script>
import 'bootstrap';
import axios from 'axios';
import 'lodash';

export default {
  name: 'App'
}
</script>

<style lang="scss">
@import "~bootstrap/scss/bootstrap";
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 61px;
}
</style>
