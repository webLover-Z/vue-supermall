<template>
  <div class="cart-list">
     <!-- <li v-for="item in cartList">{{item}}</li> -->
     <scroll class="content">
       <cart-list-item v-for="(item, index) in cartList" :key="index" :item-info="item"/>
     </scroll>
  </div>
</template>

<script>

import CartListItem from './CartListItem'

import Scroll from 'components/scroll/Scroll'

import { mapGetters } from 'vuex'

export default {
  name: 'CartList',
  components: {
    Scroll,
    CartListItem
  },
  computed: {
    ...mapGetters(['cartList'])
  }
}

</script>


<style scoped>
  .cart-list {
    height: calc(100% - 44px - 49px - 40px);
  }
  
  .content {
    overflow: hidden;
    height: 100%;
    /* position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0; */
  }
  
</style>