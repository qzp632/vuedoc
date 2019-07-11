<template>
  <div class="goods">
      <div class="menu-wrapper">
        <ul>
          <li v-for="(item, index) in goods" :key="index" class="menu-item">
            <span class="text border-1px">
              <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
            </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper">
        <ul>
          <li v-for="(item, index) in goods" :key="index" class="food-list">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li v-for="(food, index) in item.foods" :key="index" class="food-item">
                <div class="icon">
                  <img :src="food.icon" width="57" height="57">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}</span>
                    <span>好评率{{food.rating}}%</span>
                    <div class="price">
                      <span class="now">￥{{food.price}}</span>
                      <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                    </div>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
  </div>
</template>
<script>
const ERR_OK = 0
export default {
  name: 'goods',
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: []
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    this.$http.get('/api/goods').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.goods = response.data
        console.log(this.goods)
      }
    })
  }
}
</script>
<style lang="less" scoped>//stylus less
@import '../../common/styless/mixin.less';
@import './goods.less';
// @import '../../common/styless/mixin.styl';
// @import './goods.styl';
</style>
