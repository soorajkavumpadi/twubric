
<template>
  <div id="app">
    <ListUserDetails msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Vue from 'vue';
import ListUserDetails from './components/ListUserDetails.vue'
// eslint-disable-next-line no-unused-vars
import _ from 'lodash';
import moment from 'vue-moment';



Vue.use(moment);
Vue.prototype.$_ = _;
export default {
  name: 'App',
  components: {
    ListUserDetails
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
