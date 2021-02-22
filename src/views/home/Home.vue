<template>
  <div id="home">
    <nav-bar class="home-navbar"><div slot="center">购物街</div></nav-bar>
    <tab-control  :titles="['流行','新款','精选']"  
                  @tabClick="tabClick"
                  ref="tabControl1"
                  class="tabcontrol"
                  v-show="isTabFixed"/>
    <scroll class="content" 
            ref="scroll" 
            :probe-type="3" 
            @scroll="contentScroll" 
            :pull-up-load="true" @pullingUp="loadMore">

      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>
      <recommend-view :recommends="recommends"/>
      <feature-view/>
      <tab-control :titles="['流行','新款','精选']"  
                   @tabClick="tabClick"
                   ref="tabControl2"/>
      <goods-list :goods="showGoods"/>
    </scroll>
    
    <back-top @click.native="backClick" v-show="isShowBackTop"/>

  </div>
</template>

<script>
  import NavBar from 'components/navbar/NavBar'
  import TabControl from 'components/tabControl/TabControl'
  import GoodsList from 'components/goods/GoodsList'
  import GoodsListItem from 'components/goods/GoodsListItem'
  import Scroll from 'components/scroll/Scroll'
  import BackTop from 'components/backtop/BackTop'

  import HomeSwiper from './childCop/HomeSwiper'
  import RecommendView from './childCop/RecommendView'
  import FeatureView from './childCop/FeatureView'

  import {getHomeMultidata, getHomeGoods} from 'network/home'

export default {
  name: 'Home',
  components: {
    NavBar,
    TabControl,
    GoodsList,
    GoodsListItem,
    Scroll,
    BackTop,
    HomeSwiper,
    RecommendView,
    FeatureView,
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        'pop': {page: 0, list: []},
        'new': {page: 0, list: []},
        'sell': {page: 0, list: []},
      },
      currentType: 'pop',
      isShowBackTop: false,
      tabOffsetTop: 0,
      isTabFixed: false,
      saveY: 0
    }
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list
    },
    activated() {
      this.$refs.scroll.scrollTo(0, this.saveY, 0)
      this.$refs.scorll.refresh()
    },
    deactivated() {
      this.saveY = this.$refs.scroll.scorll.y
    }
  },
  created() {
    this.getHomeMultidata(),
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')

    
  },

  mounted() {
  //监听item中图片加载完成

  const refresh = this.debounce(this.$refs.scroll.refresh, 200)
    this.$bus.$on('itemImageLoad', () => {
      refresh()
    })
  },

  methods: {
    //防抖动
    debounce(func, delay) {
      let timer = null
      return function (...args) {
        if (timer) clearTimeout(timer)
        timer = setTimeout(() => {
          func.apply(this, args)
        }, delay)
      }
    },


    tabClick(index) {
      switch(index){
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
        this.$refs.tabControl1.currentIndex = index;
        this.$refs.tabControl2.currentIndex = index;
    },

    backClick() {
      this.$refs.scroll.scrollTo(0, 0, 500)
    },
    
    contentScroll(position) {
      // console.log(position)
      this.isShowBackTop = (-position.y) > 1000

      this.isTabFixed = (-position.y) > this.tabOffsetTop
    },

    loadMore() {
      this.getHomeGoods(this.currentType)
    },
    
    swiperImageLoad() {
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
    },


    // 网络请求相关方法

    getHomeMultidata() {
        getHomeMultidata().then(res => {
          // this.result = res;
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
    
    getHomeGoods(type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1

        this.$refs.scroll.finishPullUp()
      })
    }
  }
}

</script>


<style scoped>
  #home {
    /* padding-top: 44px; */
    height: 100vh;
    position: relative;
  }

  .home-navbar {
    background-color: var(--color-tint);
    color: #fff;
    font-size: 18px;

    /* position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9; */
  }
  
  /* .tab-control {
    position: sticky;
    top: 44px;
    z-index: 9;
  } */

  .content {
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

  .tabcontrol {
    position: relative;
    z-index: 9;
  }
  
</style>