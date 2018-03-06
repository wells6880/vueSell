<template>
  <div class="seller" ref="seller">
    <div class="seller-wrap">
      <div class="overview">
        <div class="overview-top">
          <h2 class="name">{{seller.name}}</h2>
          <v-star :size="36" :score="seller.score"></v-star>
          <span class="rating-count">{{'(' + seller.ratingCount + ')'}}</span>
          <span class="sell-count">月售{{seller.sellCount}}单</span>
          <div class="collection">
            <div class="icon-favorite" @click="collect()" :class="{active:isCollected}"></div>
            <div class="text">已收藏</div>
          </div>
        </div>
        <div class="overview-bottom">
          <div class="overview-item">
            <div class="text">起送价</div>
            <div class="num">
              {{seller.minPrice}}
              <span class="unit">元</span>
            </div>
          </div>
          <div class="overview-item">
            <div class="text">商家配送</div>
            <div class="num">
              {{seller.deliveryPrice}}
              <span class="unit">元</span>
            </div>
          </div>
          <div class="overview-item">
            <div class="text">评价配送时间</div>
            <div class="num">
              {{seller.deliveryPrice}}
              <span class="unit">分钟</span>
            </div>
          </div>
        </div>
      </div>
      <v-split></v-split>
      <div class="supports">
        <h2 class="title">公告与活动</h2>
        <div class="bulletin">{{seller.bulletin}}</div>
        <div class="supports-list">
          <ul>
            <li class="support" v-for="item in seller.supports">
              <v-supportsicon :supportType="item.type" :sizeType="4"></v-supportsicon>
              {{item.description}}
            </li>
          </ul>
        </div>
      </div>
      <v-split></v-split>
      <div class="pics">
        <h2 class="title">商家实景</h2>
        <div class="pics-wrap" ref="picsWrap">
          <ul ref="picsList">
            <li class="pic" v-for="item in seller.pics">
              <img :src="item">
            </li>
          </ul>
        </div>
      </div>
      <v-split></v-split>
      <div class="infos">
        <h2 class="title">商家信息</h2>
        <ul>
          <li class="info" v-for="item in seller.infos">
            {{item}}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import vSplit from '../split/split.vue'
import vStar from '../star/star.vue'
import vSupportsicon from '../supportsicon/supportsicon.vue'
import BScroll from 'better-scroll';

export default{
  props:{
    seller:{
      type:Object
    }
  },
  components:{
    vSplit,
    vStar,
    vSupportsicon
  },
  data(){
    return {
      isCollected:false
    }
  },
  methods:{
    collect(){
      this.isCollected = !this.isCollected
    },
    _initScroll(){
      if(!this.sellerScroll){
        this.sellerScroll = new BScroll(this.$refs.seller,{
          scrollX:true
        })
      }
      else {
        this.sellerScroll.refresh();
      }
    },
    _initPicsScroll(){
      if(this.seller.pics){
        let picWidth = 120;
        let margin = 6;
        let picsWidth = (picWidth + margin) * this.seller.pics.length - margin;
        this.$refs.picsList.style.width = picsWidth + 'px';
        this.$nextTick(() => {
          if(!this.picsScroll){
            this.picsScroll = new BScroll(this.$refs.picsWrap,{
              scrollX:true,
            })
          }
          else {
            this.picsScroll.refresh();
          }
        })
      }
    }
  },
  watch:{
    'seller'(){
      this.$nextTick(() => {
        this._initScroll();
        this._initPicsScroll()
      })
    }
  },
  mounted(){
    this.$nextTick(() => {
      this._initScroll();
      this._initPicsScroll()
    })
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl";

.seller
  width 100%
  overflow hidden
  position: absolute
  top: 169px
  bottom:0
  left 0
  .seller-wrap
    .overview
      padding 18px
      .overview-top
        padding-bottom 18px
        border-1px-bottom(rgba(7,17,27,.1))
        font-size 0
        position relative
        .name
          font-size 14px
          line-height 14px
          font-weight bold
          margin-bottom 8px
        .stars
          display inline-block
          padding-right 8px
        .rating-count
          font-size 10px
          line-height 15px
          color rgb(77,85,93)
          padding-right 12px
          vertical-align top
        .sell-count
          font-size 10px
          line-height 15px
          color rgb(77,85,93)
          vertical-align top
        .collection
          position absolute
          right 0
          bottom 18px
          text-align center
          .icon-favorite
            font-size 24px
            color #ccc
            &.active
              color rgb(240,20,20)
          .text
            font-size 10px
            color rgb(77,85,93)
      .overview-bottom
        padding-top 18px
        display flex
        .overview-item
          flex 1
          text-align center
          border-1px-right(rgba(7,17,27,.1))
          &:last-child
            border-right none
          .text
            font-size 10px
            line-height 10px
            color rgb(147,153,159)
            margin-bottom 4px
          .num
            font-size 24px
            line-height 24px
            color rgb(7,17,27)
            .unit
              font-size 10px
              font-weight 200
    .supports
      padding 18px 18px 0
      .title
        font-size 14px
        line-height 14px
        font-weight bold
        margin-bottom 8px
      .bulletin
        padding 12px 0 16px
        font-size 12px
        line-height 24px
        color rgb(240,20,20)
      .supports-list
        .support
          padding 16px 12px
          border-1px-top(rgba(7,17,27,.1))
          font-size 12px
          line-height 16px
          color rgb(7,17,27)
    .pics
      padding 18px
      .title
        font-size 14px
        line-height 14px
        font-weight bold
        margin-bottom 12px
      .pics-wrap
        width 100%
        overflow hidden
        white-space nowrap
        font-size 0
        .pic
          display inline-block
          margin-right 6px
          &:last-child
            margin-right 0
          img
            width 120px
            height 90px
    .infos
      padding 18px 18px 0
      .title
        font-size 14px
        line-height 14px
        font-weight bold
        margin-bottom 12px
      .info
        padding 16px 12px
        border-1px-top(rgba(7,17,27,.1))
        font-size 12px
        line-height 16px
        color rgb(7,17,27)
</style>
