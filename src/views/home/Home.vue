<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
     <tab-control
        ref="tabControl1"
        :titles="['流行', '新款', '精选']"
        @tabClick="tabClick"
        class="tab-control"
        v-show="isTabFixed"
        />

    <scroll
    class="content"
    ref='scroll'
    :probe-type="3"
    @scroll="contentScroll"
    :pull-up-load="true"
    @pullingUp="loadMore"
    >
      <home-swiper
      :banners="banners"
      @swiperImageLoad="swiperImageLoad"></home-swiper>
      <recommend-view :recommends="recommends"/>
      <feature-view />
      <tab-control
        ref="tabControl2"
        :titles="['流行', '新款', '精选']"
        @tabClick="tabClick"
        />
      <goods-list :goods="showGoods"/>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"/>
  </div>
</template>

<script>
  import HomeSwiper from './childComps/HomeSwiper.vue'
  import RecommendView from './childComps/RecommendView.vue'
  import FeatureView from './childComps/FeatureView.vue'

  import NavBar from '@/components/common/navbar/NavBar.vue'
  import TabControl from '@/components/content/tabControl/TabControl.vue'
  import GoodsList from '@/components/content/goods/GoodsList.vue'
  import Scroll from '@/components/common/scroll/Scroll.vue'
  import BackTop from '@/components/content/backTop/BackTop.vue'

  import {getHomeMultidata, getHomeGoods} from '@/network/home.js'
  import {debounce} from '@/common/util.js'

  export default {
    name: 'Home',
    components: {
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
    data () {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        },
        currentType: 'pop',
        isShowBackTop: false,
        taboffestTop: 0,
        isTabFixed: false,
        saveY: 0
      }
    },
    computed: {
      showGoods () {
        return this.goods[this.currentType].list
      }
    },
    created() {
      //1.请求多个数据
      this.getHomeMultidata()

      //2.请求商品数据
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    mounted () {
      //3.监听item中图片加载完成
      const refresh = debounce(this.$refs.scroll.refresh, 500)
      this.$bus.$on('itemImageLoad', () => {
        refresh()
      })
    },
    destroyed() {

    },
    activated() {
      this.$refs.scroll.refresh()
      this.$refs.scroll.scrollTo(0, this.saveY, 0)
    },
    deactivated() {
      this.saveY = this.$refs.scroll.getScrollY()
    },
    methods: {
      /**
       * 时间监听相关0
       */
      tabClick (index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
        this.$refs.tabControl1.currentIndex = index
        this.$refs.tabControl2.currentIndex = index
       },
      backClick () {
         // console.log(this.$refs.scroll.scroll)
         this.$refs.scroll.scrollTo(0, 0, 500)
       },
      contentScroll (position) {
        // console.log(position)
        this.isShowBackTop = (-position.y) > 1000

        this.isTabFixed = (-position.y) > this.taboffestTop
      },
      loadMore () {
        // console.log('上拉加载更多')
        this.getHomeGoods(this.currentType)
      },
      swiperImageLoad () {
        this.taboffestTop = this.$refs.tabControl2.$el.offsetTop
        console.log(this.$refs.tabControl2.$el.offsetTop)
      },
      /**
       * 网络请求相关
       */
      getHomeMultidata () {
        getHomeMultidata().then(res => {
          // console.log(res)
          this.banners = res.data.banner.list
          this.recommends = res.data.recommend.list
        })
      },
      getHomeGoods (type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then( res => {
          // console.log(res)
          this.goods[type].list.push(...res.data.list)

          //完成下拉加载更多
          this.$refs.scroll.finishPullUp()
        })
      }
    }
  }
</script>

<style scoped>
  #home {
    /* padding-top: 44px; */
    height: ;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #FFFFFF;
/*
    position: fixed;
    left: 0;
    right: 0;
    top: 0; */
    /* z-index: 9; */
  }

 .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

  .tab-control {
    position: relative;
    z-index: 9;
  }
/*  .fixed {
    position: fixed;
    left: 0;
    right: 0;
    top: 44px;
  } */

/*  .content {
    height: calc(100% - 93px);
    overflow: hidden;
    margin-top: 44px;
  } */

</style>
