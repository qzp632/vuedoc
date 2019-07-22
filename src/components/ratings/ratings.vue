<template>
    <div class="ratings" ref="ratings">
        <div class="ratings-content">
          <div class="overview">
            <div class="overview-left">
              <h1 class="score">{{seller.score}}</h1>
              <div class="title">综合评分</div>
              <div class="rank">高于周边商家{{seller.rankRate}}</div>
            </div>
            <div class="overview-right">
              <div class="score-wrapper">
                <span class="title">服务态度</span>
                <star :size="36" :score="seller.serviceScore"></star>
                <span class="score">{{seller.serviceScore}}</span>
              </div>
              <div class="score-wrapper">
                <span class="title">商品评分</span>
                <star :size="36" :score="seller.foodScore"></star>
                <span class="score">{{seller.foodScore}}</span>
              </div>
              <div class="delivery-wrapper">
                <span class="title">送达时间</span>
                <span class="delivery">{{seller.deliveryTime}}分钟</span>
              </div>
            </div>
          </div>
          <split></split>
          <ratingselect @onSelect="onSelect" @onToggle="onToggle" :selectType="selectType" :onlyContent="onlyContent" :ratings="ratings"></ratingselect>
          <div class="rating-wrapper">
            <ul>
              <li v-for="(rating, index) in ratings" v-show="needShow(rating.rateType, rating.text)" :key="index" class="rating-item">
                <div class="avatar">
                  <img width="28" height="28" :src="rating.avatar">
                </div>
                <div class="content">
                  <h1 class="name">{{rating.username}}</h1>
                  <div class="star-wrapper">
                    <star :size="24" :score="rating.score"></star>
                    <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
                  </div>
                  <p class="text">{{rating.text}}</p>
                  <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                    <span class="icon-thumb_up"></span>
                    <span class="item" v-for="(item, index) in rating.recommend" :key="index">{{item}}</span>
                  </div>
                  <div class="time">{{rating.rateTime | formatDate}}</div>
                </div>
              </li>
            </ul>
          </div>
        </div>
    </div>
</template>
<script>
import BScroll from 'better-scroll'
import { formatDate } from 'common/js/date'
import star from 'components/star/star'
import split from 'components/split/split'
import ratingselect from 'components/ratingselect/ratingselect'
const ALL = 2
const ERR_OK = 0
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      ratings: [],
      selectType: ALL,
      onlyContent: true
    }
  },
  created () {
    this.$http.get('/api/ratings').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.ratings = response.data
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.ratings, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      }
    })
  },
  methods: {
    onSelect (type) {
      // alert(10)
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    onToggle () {
      // alert(0)
      this.onlyContent = !this.onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    needShow (type, text) {
      if (this.onlyContent && !text) {
        // alert(0)
        return false
      }
      if (this.selectType === ALL) {
        // alert(1)
        return true
      } else {
        // alert(2)
        return type === this.selectType
      }
    }
  },
  filters: {
    formatDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  components: {
    star,
    split,
    ratingselect
  }
}
</script>
<style lang="less" scoped>//stylus less
@import '../../common/styless/mixin.less';
@import './ratings.less';
// @import '../../common/styless/mixin.styl';
// @import './ratings.styl';
</style>
