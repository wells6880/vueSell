<template>
  <transition name="from-right">
    <div class="food" v-show="isFoodShow" ref="food">
      <div class="food-wrap">
        <div class="img">
          <img :src="food.image" alt="图片">
        </div>
        <div class="food-content">
          <h2 class="food-name">{{food.name}}</h2>
          <div class="food-extra">
        <span class="food-extra-sell-count">
          月售{{food.sellCount}}份
        </span>
            <span class="food-extra-rating">
          好评率{{food.rating}}%
        </span>
          </div>
          <div class="food-price">
            <div class="price-now">
              <span class="unit">￥</span>
              {{food.price}}
            </div>
            <div class="price-old" v-if="food.oldPrice">
              <span class="unit">￥</span>
              {{food.oldPrice}}
            </div>
          </div>
          <div class="add-wrap">
            <div class="add-btn" @click="foodNumAdd(food,$event)" v-if="!food.count">加入购物车</div>
            <v-cartcontrol v-if="food.count" :food="food" @add="addFood"></v-cartcontrol>
          </div>
        </div>
        <v-split></v-split>
        <div class="info">
          <h2 class="title">商品介绍</h2>
          <p class="text">{{food.info}}</p>
        </div>
        <v-split></v-split>
        <div class="ratings">
          <h2 class="title">商品评价</h2>
          <v-ratingselect :ratings="food.ratings" :ratingsTypeNum="1" @select="selectRating()"></v-ratingselect>
        </div>
        <div class="icon-arrow_lift" @click="close()"></div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
import vSplit from '../split/split.vue'
import vRatingselect from '../ratingselect/ratingselect.vue'
import BScroll from 'better-scroll';
import Vue from 'vue'
import vCartcontrol from '../cartcontrol/cartcontrol.vue'

export default {
  props:{
    food:{
      type:Object
    },
  },
  components:{
    vSplit,
    vRatingselect,
    vCartcontrol
  },
  data(){
    return {
      isFoodShow:false
    }
  },
  methods:{
    show(){
      this.isFoodShow = true;
      this.$nextTick(() => {
        if(!this.foodScroll){
          this.foodScroll = new BScroll(this.$refs.food,{
            click:true
          });
        }
        else {
          this.foodScroll.refresh();
        }
      })
    },
    close(){
      this.isFoodShow = false;
    },
    selectRating(){
      this.$nextTick(() => {
        this.foodScroll.refresh();
      })
    },
    foodNumAdd(food,event){
      this.$emit('add', event.target);
      Vue.set(this.food, 'count', 1);
    },
    addFood(target){
      this._drop(target)
    },
    _drop(target){
      //异步加载，优化动画
      this.$nextTick(() => {
        this.$emit('add',target);
      })
    },
  }
}
</script>

<style lang="stylus">
.food
  position absolute
  left 0
  top 0
  bottom 46px
  width 100%
  background #fff
  overflow hidden
  z-index 10
  &.from-right-enter,&.from-right-leave-to
    transform translate(100%,0)
  &.from-right-enter-active,&.from-right-leave-active
    transition all 0.4s
  .food-wrap
    .img
      img
        width 100%
        height 325px
    .food-content
      flex 1
      margin-left 10px
      position relative
      .food-name
        font-size 14px
        font-weight 700
        color rgb(7,17,27)
        margin-top 2px
      .food-description
        font-size 10px
        line-height 14px
        color rgb(147,153,159)
        margin-top 8px
      .food-extra
        font-size 10px
        color rgb(147,153,159)
        margin-top 8px
        overflow hidden
        .food-extra-sell-count
          float left
        .food-extra-rating
          float left
          margin-left 12px
      .food-price
        margin-top 6px
        font-size 10px
        color red
        font-weight normal
        line-height 24px
        overflow hidden
        .price-now
          float left
          font-size 14px
          font-weight 700
        .price-old
          float left
          font-weight 700
          text-decoration: line-through
          margin-left 8px
          color rgb(147,153,159)
        .unit
          font-size 10px
          font-weight normal
      .add-wrap
        position absolute
        right 18px
        bottom 18px
        .add-btn
          width 74px
          height 24px
          line-height 24px
          font-size 10px
          text-align center
          border-radius 12px
          color #fff
          background rgb(0,160,220)
    .info
      padding 18px
      .title
      .text
        font-size 12px
        font-weight 200
        line-height 24px
        color rgb(77,85,93)
    .ratings
      .title
        padding 18px 0 6px 18px
    .icon-arrow_lift
      position absolute
      left 0
      top 0
      color #fff
      padding 15px
</style>
