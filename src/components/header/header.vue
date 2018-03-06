<template>
  <div class="header">
    <div class="warp">
      <img class="img" :src="seller.avatar">
      <div class="content">
        <div class="name">
          <span class="brand-icon"></span>{{seller.name}}
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="supports">
          <v-supportsicon :supportType="seller.supports[0].type" :sizeType="1"></v-supportsicon>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="supports-count" @click="showDetail()">
        {{seller.supports.length}}个<i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin" @click="showDetail()">
      <span class="icon"></span><span class="bulletin-content">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="bg">
      <img :src="seller.avatar">
    </div>
    <transition name="fade">
      <div class="detail" v-if="isShowDetail">
        <div class="detail-wrap">
          <div class="detail-main">
            <h2 class="detail-name">
              {{seller.name}}
            </h2>
            <v-star :score="seller.score" :size="48"></v-star>
            <div class="title-wrap">
              <div class="detail-title">
                <span class="line"></span>
                <span class="text">优惠信息</span>
                <span class="line"></span>
              </div>
              <div class="detail-supports">
                <ul>
                  <li v-for="item in seller.supports" class="supports-item">
                    <v-supportsicon :supportType="item.type" :sizeType="2"></v-supportsicon>
                    <span class="text">{{item.description}}</span>
                  </li>
                </ul>
              </div>
            </div>
            <div class="title-wrap">
              <div class="detail-title">
                <span class="line"></span>
                <span class="text">商家公告</span>
                <span class="line"></span>
              </div>
              <div class="detail-bulletin">
                {{seller.bulletin}}
              </div>
            </div>
          </div>
        </div>
        <div class="close">
          <i class="icon-close" @click="close()"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
import vStar from '../star/star.vue'
import vSupportsicon from '../supportsicon/supportsicon.vue'

export default{
  props: {
    seller: {
      type: Object
    }
  },
  components:{
    vStar,
    vSupportsicon
  },
  data(){
    return{
      isShowDetail:false
    }
  },
  methods:{
    showDetail(){
      this.isShowDetail = true;
    },
    close(){
      this.isShowDetail = false;
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl";

.header
  width 100%
  color #fff
  .warp
    padding 20px
    font-size 14px
    overflow hidden
    position relative
    background rgba(7,17,27,.5)
    .img
      float left
      width 60px
      height 60px
    .content
      float left
      margin-left 20px
      .name
        font-size 16px
        font-weight 550
        line-height 18px
        .brand-icon
          display inline-block
          width 30px
          height 18px
          bg-img('brand')
          background-size cover
          vertical-align top
          margin-right 6px
      .description
        margin 8px 0 10px
        font-size 12px
        font-weight 200
        line-height 12px
      .supports
        font-size 0
        .text
          display inline-block
          font-size 10px
          line-height:12px
          font-weight 200
    .supports-count
      position absolute
      right 20px
      bottom 20px
      padding 5px 10px
      border-radius 10px
      font-size 12px
      background rgba(10,10,10,.5)
  .bulletin
    font-size 10px
    padding 0 36px 0 24px
    background rgba(10,10,10,.8)
    overflow: hidden;
    text-overflow:ellipsis;
    white-space: nowrap;
    position: relative
    .icon
      display inline-block
      width 22px
      height 12px
      margin 8px 4px 0 0
      bg-img('bulletin')
      background-size cover
      vertical-align top
    .bulletin-content
      height 28px
      line-height 28px
    .icon-keyboard_arrow_right
      position absolute
      right 22px
      top 0
      height 28px
      line-height 28px
  .bg
    width 100%
    height 128px
    position absolute
    left 0
    top 0
    z-index -1
    filter: blur(10px)
    img
      width 100%
      height 100%
  .detail
    width 100%
    height 100%
    position fixed
    left 0
    top 0
    background rgba(7,17,27,.8)
    opacity 1
    backdrop-filter: blur(10px)
    z-index 100
    overflow: auto
    &.fade-enter-active, &.fade-leave-active
      transition all 0.5s
    &.fade-enter, &.fade-leave-to
      opacity 0
      background rgba(7, 17, 27, 0)
    .detail-wrap
      overflow hidden
      min-height 100%
      .detail-main
        padding-bottom 64px
        .detail-name
          font-size 16px
          font-weight 700
          color #fff
          text-align center
          margin 64px 0 16px
        .title-wrap
          margin 0 36px
          font-size 0
          .detail-title
            margin 28px 0 24px 0
            display flex
            .text
              font-size 14px
              font-weight 700
              color #fff
              margin 0 12px
            .line
              flex 1
              display inline-block
              height 1px
              background rgba(255,255,255,.2)
              margin 6.5px 0
          .detail-supports
            font-size 12px
            font-weight 200
            color #fff
            margin 0 12px
            .supports-item
              font-size 0
              .text
                font-size 12px
                font-weight 200
                color #fff
                line-height 16px
            .supports-item + .supports-item
              margin-top 12px
          .detail-bulletin
            font-size 12px
            font-weight 200
            color #fff
            line-height 24px
            margin 0 12px
    .close
      text-align center
      width 100%
      font-size 32px
      color rgba(255,255,255,.5)
      margin-top -64px
      line-height 40px
</style>
