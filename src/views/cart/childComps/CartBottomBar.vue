<template>
  <div class="bottom-bar">
    <div class="check-content">
      <check-button :is-checked="isSelectAll" class="check-button" @click.native="checkClick"/>
      <span>全选</span>
    </div>

    <div class="price">
    合计: {{totalPrice}}
    </div>

    <div class="calculate">
      去结算({{checkLength}})
    </div>
  </div>
</template>

<script>

import CheckButton from 'components/checkbuttom/CheckButton'

import { mapGetters } from 'vuex'

export default {
  name: 'CartBottomBar',
  components: {
    CheckButton
  },
  computed: {
    ...mapGetters(['cartList']),
    totalPrice() {
      return '￥' + this.cartList.filter(item => {
        return item.checked
      }).reduce((preValue, item) => {
        return preValue + Number(item.price.substr(1)) * item.count
          // return console.log(typeof item.price)
      }, 0)
    },
    checkLength() {
      return this.cartList.filter(item => item.checked).length
    },
    isSelectAll() {
      if (this.cartList.length === 0) return false
        return !this.cartList.find(item => !item.checked)
    }
  },
  methods: {
    checkClick() {
      if (this.isSelectAll) {
        this.cartList.forEach(item => item.checked = false)
      } else {
        this.cartList.forEach(item => item.checked = true)
      }
    }

  }
}
// .toFixed(2)
</script>


<style>
  .bottom-bar {
    height: 40px;
    position: relative;
    background-color: #eee;
    line-height: 40px;
    display: flex;
  }
  .check-content {
    display: flex;
    align-items: center;
    margin-left: 10px;
    width: 60px;
  }
  .check-button {
    width: 20px;
    height: 20px;
    line-height: 20px;
    margin-right: 5px;
  }
  .check-content span {
    font-size: 16px;
  }
  .price {
    font-size: 16px;
    margin-left: 15px;
    flex: 1;
  }
  .calculate {
    font-size: 16px;
    margin-left: 15px;
    width: 90px;
    background-color: orangered;
    color: white;
    text-align: center;
  }
</style>