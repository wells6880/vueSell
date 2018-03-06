<template>
  <div>
    <div class="ratings" ref="ratings">
      <div class="ratings-content">
        <div class="overview">
          <div class="overview-left">
            <div class="score">{{seller.score}}</div>
            <div class="title">综合评分</div>
            <div class="rank">高于周边商家{{seller.rankRate}}%</div>
          </div>
          <div class="overview-right">
            <div class="overview-right-wrap">
              <div class="score-wrap">
                <span class="title">服务态度</span>
                <v-star :score="seller.serviceScore" :size="36"></v-star>
                <span class="score">{{seller.serviceScore}}</span>
              </div>
              <div class="score-wrap">
                <span class="title">商品评分</span>
                <v-star :score="seller.foodScore" :size="36"></v-star>
                <span class="score">{{seller.foodScore}}</span>
              </div>
              <div class="score-wrap">
                <span class="title">送达时间</span>
                <span class="time">{{seller.deliveryTime}}分钟</span>
              </div>
            </div>
          </div>
        </div>
        <v-split></v-split>
        <v-ratingselect :ratings="ratings" :ratingsTypeNum="0" @select="selectRating()"></v-ratingselect>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import vStar from '../star/star.vue';
import BScroll from 'better-scroll';
import vRatingselect from '../ratingselect/ratingselect.vue'
import vSplit from '../split/split.vue'

export default{
  components:{
    vStar,
    vRatingselect,
    vSplit
  },
  props:{
    seller:{
      type:Object
    }
  },
  created(){
    this.$http.get('/api/ratings').then((response) => {
      response = response.body;
      if(response.errno === 0){
        this.ratings = response.data;
        this.$nextTick(() => {
          this.ratingsScroll = new BScroll(this.$refs.ratings,{
            click:true
          })
        })
      }
    })
  },
  data(){
    return{
      ratings:[],
    }
  },
  methods:{
    selectRating(){
      this.$nextTick(() => {
        this.ratingsScroll.refresh();
      })
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
@import "../../common/stylus/mixin.styl";

.ratings
  width 100%
  position: absolute
  top: 169px
  bottom 0
  left 0
  overflow hidden
  .overview
    display flex
    margin 18px 0
    .overview-left
      flex 1
      text-align center
      border-1px-right(rgba(7,17,27,.1))
      .score
        font-size 24px
        color rgb(255,153,0)
        line-height 28px
      .title
        font-size 12px
        color #000
        margin-top 6px
      .rank
        font-size 10px
        color rgb(147,153,159)
        margin-top 8px
    .overview-right
      flex 1.5
      font-size 0
      .overview-right-wrap
        width 192px
        margin 0 auto
        .score-wrap
          margin-bottom 8px
          &:last-child
            margin none
        .title
          font-size 12px
          color rgb(7,17,27)
          line-height 18px
          margin-right 12px
        .stars
          display inline-block
          margin-right 12px
          vertical-align top
        .score
          font-size 12px
          color rgb(255,153,0)
          line-height 18px
        .time
          font-size 12px
          color rgb(147,153,159)
          line-height 18px
</style>
