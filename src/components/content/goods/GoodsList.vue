<template>
  <div class="goods">
    <goods-list-item
    class="goods-item"
    v-for="(item, index) in goods"
    :key="index"
    :goods-item="item" />
  </div>
</template>

<script>
  import GoodsListItem from './GoodsListItem.vue'

  export default {
    name: 'GoodsList',
    components: {
      GoodsListItem
    },
    props: {
      goods: {
        type: Array,
        default() {
          return []
        }
      }
    }
  }
</script>

<style>
  .goods {
    display: flex;
    flex-warp: wrap;
    justify-content: space-around;
  }
</style>
