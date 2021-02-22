<template>
  <div class="toast" v-show="isShow">
    {{message}}
  </div>
</template>

<script>
export default {
  name: 'Toast',
  props: {
    // message: {
    //   type: String,
    //   default: ''
    // },
    // show: {
    //   type: Boolean,
    //   default: false
    // }
  },
  data() {
    return {
      message: '',
      isShow: false
    }
  },
  methods: {
    show(message, duration) {
      
      // console.log('111')
      
      this.isShow = true;
      this.message = message

      setTimeout(() => {
        this.isShow = false;
        this.message = ''
      }, duration)
    }
  }
}

</script>


<style scoped>
  .toast {
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    padding: 8px 10px;
    border-radius: 10px;
    
    z-index: 999;

    color:#fff;
    background-color: rgba(0,0,0, .75);
    
    font-size: 18px;
  }
</style>