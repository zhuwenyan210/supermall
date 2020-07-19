<template>
  <div id="detail">
    <detail-nav-bar />
    <scroll class="content" ref="scroll">
      <detail-swiper :top-images="topImages"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
      <detail-goods-info
        :detail-info="detailInfo"
        @imageLoad="imageLoad"/>
      <detail-param-info :param-info="paramInfo"/>
    </scroll>
  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar.vue'
  import DetailSwiper from './childComps/DetailSwiper.vue'
  import DetailBaseInfo from './childComps/DetailBaseInfo.vue'
  import DetailShopInfo from './childComps/DetailShopInfo.vue'
  import DetailGoodsInfo from './childComps/DetailGoodsInfo.vue'
  import DetailParamInfo from './childComps/DetailParamInfo.vue'

  import Scroll from '@/components/common/scroll/Scroll.vue'

  import {getDetail, Goods, Shop, GoodsParam} from '@/network/detail.js'

  export default {
    name: 'Detail',
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      Scroll,
      DetailGoodsInfo,
      DetailParamInfo
    },
    data () {
      return {
        iid: null,
        topImages: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {}
      }
    },
    created() {
      //1、保存iid
      // console.log(this.$route.params)
      this.iid = this.$route.params.iid
      //2、根据iid请求数据
      getDetail(this.iid).then(res => {
        //获取顶部的图片轮播图数据
        console.log(res)
        const data = res.result
        this.topImages = data.itemInfo.topImages
        // console.log(this.topImages)
        //获取商品信息
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
        console.log(this.goods)

        //3，创建店铺信息的对象
        this.shop = new Shop(data.shopInfo)

        //4、获取商品详细信息
        this.detailInfo = data.detailInfo

        //5、获取参数
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

      })
    },
    methods: {
      imageLoad () {
        this.$refs.scroll.refresh()
      }
    }
  }
</script>

<style scoped>
  #detail {
    height: 100vh;
    position: relative;
    z-index: 1;
    /* background-color: #fff; */
  }
  .content {
/*    position: absolute;
    top: 44px;
    bottom: 60px; */
    height: 600px;
    overflow: hidden;
  }
  .back-top {
    position: fixed;
    right: 10px;
    bottom: 65px;
  }
</style>
