<template>
  <div>
    <v-header :seller="seller"></v-header>

    <div class="tab border-1px">
      <div class="tab-item">
        <router-link :to="{path: '/goods'}">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link :to="{path: '/ratings'}">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link :to="{path: '/seller'}">商家</router-link>
      </div>
    </div>

    <router-view :seller="seller"></router-view>
  </div>
</template>
<script>
import header from './components/header/header'
const ERR_OK = 0
export default {
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$http.get('/api/seller').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.seller = response.data
      }
    })
  },
  components: {
    'v-header': header
  }
}
</script>
<style lang="less" scoped>//stylus less
  // @import "./common/styless/mixin.styl";
  @import "./common/styless/mixin.less";
  .tab {
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    .border-1px(rgba(7,17,27,0.1));
    .tab-item {
      flex: 1;
      text-align: center;
      & > a {
        display: block;
        font-size: 14px;
        color: rgb(77,89,93);
        &.active {
          color: rgb(240,20,20);
        }
      }
    }
  }
  // .tab
  //   display: flex
  //   width: 100%
  //   height: 40px
  //   line-height: 40px
  //   border-1px(rgba(7,17,27,0.1))
  //   .tab-item
  //     flex: 1
  //     text-align: center
  //     & > a
  //       display: block
  //       font-size: 14px
  //       color: rgb(77,89,93)
  //       &.active
  //         color: rgb(240,20,20)
</style>
