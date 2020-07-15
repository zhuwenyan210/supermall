<template>
  <div id="app">
    <mian-tab-bar></mian-tab-bar>
    <router-view></router-view>

  </div>
</template>

<script>
  import MianTabBar from '@/components/content/mainTabBar/MainTabBar.vue'

  export default {
    name: 'App',
    components: {
      MianTabBar
    }
  }
</script>

<style>
  @import url("assets/css/base.css");
</style>
