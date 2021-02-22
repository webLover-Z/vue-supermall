<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"/>
    <scroll class="content" ref="scroll" 
            @scroll="contentScroll" 
            :probe-type="3">
      <detail-swiper :top-images="topImages"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
      <detail-images-info :images-info="detailInfo" @imageLoad="imageLoad"/>
      <detail-param-info :param-info="paramInfo" ref="param"/>
      <detail-comment-info :comment-info="commentInfo" ref="comment"/>
      <goods-list :goods="recommends" ref="recommend" />
    </scroll>
    <detail-bottom-bar @addCart="addToCart"/>
    <back-top @click.native="backClick" v-show="isShowBackTop"/>
    <!-- <toast :message="message" :show="show"/> -->
  </div>
  
</template>

<script>
import DetailNavBar from './childComps/DetailNavBar'
import DetailSwiper from './childComps/DetailSwiper'
import DetailBaseInfo from './childComps/DetailBaseInfo'
import DetailShopInfo from './childComps/DetailShopInfo'
import DetailImagesInfo from './childComps/DetailImagesInfo'
import DetailParamInfo from './childComps/DetailParamInfo'
import DetailCommentInfo from './childComps/DetailCommentInfo'
import DetailBottomBar from './childComps/DetailBottomBar'

import Scroll from 'components/scroll/Scroll.vue'
import GoodsList from 'components/goods/GoodsList'

import Toast from 'components/Toast/Toast'

import {getDetail, Goods, Shop, GoodsParam, getRecommend} from 'network/detail'
import { debounce } from '../../common/utils'
import { backTopMixin } from '../../common/mixin'

export default {
  name: 'Detail',
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    DetailImagesInfo,
    DetailParamInfo,
    DetailCommentInfo,
    DetailBottomBar,
    Scroll,
    GoodsList,
    // Toast
  },
  mixins: [backTopMixin],
  data() {
    return {
      iid: null,
      topImages: [],
      goods: {},
      shop: {},
      detailInfo: {},
      paramInfo: {},
      commentInfo: {},
      recommends: [],
      themeTopY: [],
      getThemeTopY: null,
      currentIndex: 0,
      // message: '',
      // show: false
    }
  },
  created() {
    //保存传入的id
    this.iid = this.$route.params.iid
    //球球详情数据


    getDetail(this.iid).then(res => {
      //获取顶部轮播图的信息
      console.log(res);
      const data = res.result;
      this.topImages = data.itemInfo.topImages;

      //获取商品信息
      this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services);

      //创建店铺信息的对象
      this.shop = new Shop(data.shopInfo);

      //保存商品详情数据
      this.detailInfo = data.detailInfo;

      //获取参数信息
      this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule);

      //取出评论信息
      if (data.rate.cRate !== 0) {
        this.commentInfo = data.rate.list[0]
      }
    }) 
  
    //请求推荐数据
    getRecommend().then(res => {
      this.recommends = res.data.list
    })

    // this.getThemeTopY = debounce(() => {

    //   // this.themeTopYs = [];
		//   // this.themeTopYs.push(0);
		//   // this.themeTopYs.push(this.$refs.param.$el.offsetTop);
		//   // this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
		//   // this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);
    //   // console.log(this.themeTopYs)
    // })
  },
  
  mounted() {
      // this.themeTopY = [];
			
			// this.themeTopY.push(Number.MAX_VALUE);
  },
  updated() {
    
  },
  methods: {
    imageLoad() {
      this.$refs.scroll.refresh();

			this.themeTopY = [];
    
      this.themeTopY.push(0);
		  this.themeTopY.push(this.$refs.param.$el.offsetTop - 44);
		  this.themeTopY.push(this.$refs.comment.$el.offsetTop - 44);
		  this.themeTopY.push(this.$refs.recommend.$el.offsetTop - 44);
      // console.log(this.themeTopY)

      
		  


      // this.getThemeTopY()
      // this.themeTopYs = []

      // this.themeTopYs.push(0);
      // this.themeTopYs.push(this.$refs.param.$el.offsetTop);
      // this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
      // this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);

      
    },
    titleClick(index) {
      console.log(index)
      // console.log(this.$refs.scroll.scroll)
      this.$refs.scroll.scrollTo(0, -this.themeTopY[index], 500)
      // console.log(this.themeTopY)
    },

    contentScroll(position) {
      // console.log(position);

      const positionY = -position.y

      let length = this.themeTopY.length
      for(let i = 0; i < length; i++) {
        if(this.currentIndex !== i && ((i < length - 1 && positionY >= this.themeTopY[i] && positionY < this.themeTopY[i+1])
         || (i === length - 1 && positionY >= this.themeTopY[i]))){
              this.currentIndex = i;
              // console.log(this.currentIndex)
              this.$refs.nav.currentIndex = this.currentIndex
        }
      }

      this.listenShowBackTop(position)
    },

    addToCart() {
      //获取购物车需要展示信息
      const product = {}
     
      product.image = this.topImages[0];
      product.title = this.goods.title;
      product.desc = this.goods.desc;
      product.price = this.goods.newPrice;
      product.iid = this.iid;

      //将商品添加到购物车里
      this.$store.dispatch('addCart', product).then(res => {
        // this.show = true
        // this.message = res
        
        // setTimeout(() => {
        //   this.show = false
        //   this.message = ''
        // }, 1500);
        // this.$toast.show(res, 2000)
        this.$toast.show(res, 1500)
        // console.log(this.$toast)
      })
    }
  }
}

</script>


<style scoped>
 #detail {
   position: relative;
   z-index: 9;
   background-color: #fff;
   height: 100vh;
 }

 .detail-nav {
   position: relative;
   z-index: 9;
   background-color: #fff;
 }
 
 .content {
   height: calc(100% - 44px);
 }

</style>