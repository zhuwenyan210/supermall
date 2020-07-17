<template>
  <div class="wrapper" ref="aaa">
    <ul class="content">
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
      <li>12121</li>
    </ul>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: 'Category',
    data() {
      return {
        scroll: null
      }
    },
    mounted () {
     let wrapper = document.querySelector('.wrapper')
     this.scroll = new BScroll(wrapper, {
         pullUpLoad: true,
         probeType: 3
     })

     // this.scroll.on('scroll', (position) => {
     //   console.log(position)
     // })

     this.scroll.on('pullingUp', () => {
       console.log('上拉加载更多')
     })
    }
  }
</script>

<style scoped>
  .wrapper {
    height: 150px;
    background-color: #FF5777;

    overflow: hidden;
    /* overflow-y: scroll; */
  }
</style>
