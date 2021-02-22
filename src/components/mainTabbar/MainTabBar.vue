<template>
  <tab-bar>
    <tab-bar-item path="/home" activeColor="red">
      <img slot="item-icon" src="~assets/img/tabbar/home.png" alt="">
      <img slot="item-icon-active" src="~assets/img/tabbar/home_active.png" alt="">
      <div slot="item-text">首页</div>
    </tab-bar-item>
    <tab-bar-item path="/category">
      <img slot="item-icon" src="~assets/img/tabbar/fenlei.png" alt="">
      <img slot="item-icon-active" src="~assets/img/tabbar/fenlei_active.png" alt="">
      <div slot="item-text">分类</div>
    </tab-bar-item>
    <tab-bar-item path="/cart">
      <img slot="item-icon" src="~assets/img/tabbar/shopcart.png" alt="">
      <img slot="item-icon-active" src="~assets/img/tabbar/shopcart_active.png" alt="">
      <div slot="item-text">购物车</div>
    </tab-bar-item>
    <tab-bar-item path="/profile">
      <img slot="item-icon" src="~assets/img/tabbar/mine.png" alt="">
      <img slot="item-icon-active" src="~assets/img/tabbar/mine_active.png" alt="">
      <div slot="item-text">我的</div>
    </tab-bar-item>
  </tab-bar>
</template>

<script>

import TabBar from '../Tabbar/TabBar'
import TabBarItem from '../Tabbar/TabBarItem'

export default {
  name: 'MainTabBar',
  components: {
    TabBar,
    TabBarItem
  }
}

</script>


<style>

</style>