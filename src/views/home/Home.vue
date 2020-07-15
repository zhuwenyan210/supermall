<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from '@/components/common/navbar/NavBar.vue'
  import {getHomeMultidata} from '@/network/home.js'

  export default {
    name: 'Home',
    data () {
      return {
        banners: [],
        recommends: []
      }
    },
    components: {
      NavBar
    },
    created() {
      getHomeMultidata().then(res => {
        console.log(res)
        this.banners = res.data.banner
        this.recommends = res.data.recommend
      })
    }
  }
</script>

<style scoped>
  .home-nav {
    background-color: var(--color-tint);
    color: #F6F6F6;
  }
</style>
