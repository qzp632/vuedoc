<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count > 0" @click.stop.prevent="decreaseCart($event)">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart($event)"></div>
  </div>
</template>
<script>
import Vue from 'vue'
export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart (event) {
      if (!event._constructed) {
        return
      }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
        this.food.count = 1
      } else {
        this.food.count++
      }
      this.$emit('shopCartClick', event.target)
    },
    decreaseCart (event) {
      if (!event._constructed) {
        return
      }
      if (this.food.count) {
        this.food.count--
      }
    }
  }
}
</script>
<style lang="less" scoped>//stylus less
@import '../../common/styless/mixin.less';
@import './cartcontrol.less';
// @import '../../common/styless/mixin.styl';
// @import './shopcart.styl';
</style>
