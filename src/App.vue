<template>
  <div id="app">
    <keep-alive exclude="Detail">
      <router-view></router-view>
    </keep-alive>
    <main-tab-bar></main-tab-bar>
  </div>
</template>

<script>

import MainTabBar from 'components/mainTabbar/MainTabBar.vue'
  
  export default {
    name: 'App',
    components: {
      MainTabBar
    }
  }
</script>

<style scoped>
  /* @import "./assets/css/base"; */
  @import "assets/css/base.css";
</style>
