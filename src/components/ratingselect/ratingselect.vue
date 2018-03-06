<template>
  <div class="rating-select">
    <div class="select">
      <div class="ratings-btn">
            <span v-for="(type,index) in ratingsType[ratingsTypeNum]"
                  class="btn" @click="selectType(index)"
                  :class="{'active':selectIndex === index}">
              {{type + clacTypeNum(index).length}}</span>
      </div>
      <div class="ratings-select" @click="check()">
        <i class="icon-check_circle" :class="{'active':checked}"></i>
        <span class="text">只看有内容的评价</span>
      </div>
    </div>
    <div class="details">
      <ul>
        <li class="details-item" v-for="rating in ratings" v-if="isShowDetail(rating.rateType,rating.text)">
          <div class="img">
            <img :src="rating.avatar">
          </div>
          <div class="content">
            <div class="username">{{rating.username}}</div>
            <v-star :score="rating.score" :size="24"></v-star>
            <span class="deliveryTime">{{rating.deliveryTime}}分钟送达</span>
            <div class="text">{{rating.text}}</div>
            <div class="recommend">
              <i class="icon-thumb_up"></i>
              <span class="recommend-item" v-for="item in rating.recommend">{{item}}</span>
            </div>
          </div>
          <div class="rate-time">
            {{rating.rateTime | formatDate}}
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import vStar from '../star/star.vue';
import {formatDate} from '../../common/js/date';
import BScroll from 'better-scroll'

export default {
  props:{
    ratings:{
      type:Array
    },
    ratingsTypeNum:{
      type:Number
    }
  },
  components:{
    vStar
  },
  data(){
    return{
      ratingsType:[
        [
          '全部',
          '满意',
          '不满意'
        ],
        [
          '全部',
          '推荐',
          '吐槽'
        ]
      ],
      selectIndex:0,
      checked:true,
      newSelectIndex:2,
      typeNum:[]
    }
  },
  methods:{
    selectType(idx){
      this.selectIndex = idx;
      this.$emit('select')
    },
    check(){
      this.checked = !this.checked;
      this.$emit('select')
    },
    initTypeCode(){
      if(this.selectIndex === 0){
        this.newSelectIndex = this.ratingsType[this.ratingsTypeNum].length - 1;
      }
      else {
        this.newSelectIndex = this.selectIndex - 1;
      }
    },
    clacTypeNum(idx){
      if(!this.ratings){
        return []
      }
      if(idx === 0){
        return this.ratings.filter((rating) => {
          return this.ratings;
        })
      }
      else {
        return this.ratings.filter((rating) => {
          return rating.rateType === idx - 1;
        })
      }
    },
    isShowDetail(type,text){
      this.initTypeCode();
      //如选中只看内容，过滤掉无内容评论
      if(this.checked && !text){
        return false
      }
      //过滤后全部的评论
      if(this.newSelectIndex === this.ratingsType[this.ratingsTypeNum].length - 1){
        return true;
      }
      //对应评价的评论
      else {
        return type === this.newSelectIndex
      }
    }
  },
  filters: {
    formatDate(time) {
      let date = new Date(time);
      return formatDate(date, 'yyyy-MM-dd hh:mm');
    }
  },
}
</script>

<style lang="stylus">
@import "../../common/stylus/mixin.styl";

.rating-select
  .select
    padding 0 18px
    border-bottom 1px solid rgb(230,231,232)
    .ratings-btn
      border-1px-bottom(rgba(7,17,27,.1))
      .btn
        display inline-block
        font-size 14px
        padding 8px 10px
        margin 12px 4px
        background rgb(204,236,248)
        &.active
          background rgb(0,160,220)
          color #fff
        &:last-child
          background rgb(233,235,236)
          &.active
            background rgb(77,85,93)
            color: #fff;
    .ratings-select
      font-size 0
      padding 12px 0
      .icon-check_circle
        font-size 18px
        &.active
          color rgb(0,200,80)
      .text
        font-size 14px
        color rgb(147,153,159)
        line-height 18px
        margin-left 6px
        vertical-align top
  .details
    .details-item
      padding 18px 0
      margin 0 18px
      display flex
      position relative
      border-1px-bottom(rgba(7,17,27,.1))
      .img
        flex 0 0 28px
        margin-right 12px
        img
          width 28px
          height 28px
          border-radius 50%
      .content
        flex 1
        .username
          font-size 10px
          color rgb(7,17,27)
          line-height 12px
          margin-bottom 4px
        .stars
          display inline-block
          margin-bottom 6px
        .deliveryTime
          font-size 10px
          font-weight 200
          color rgb(147,153,159)
          line-height 12px
        .text
          font-size 12px
          color rgb(7,17,27)
          line-height 18px
          margin-bottom 8px
        .recommend
          line-height 24px
          font-size 0
          .icon-thumb_up
            font-size 12px
            color rgb(0,160,220)
            line-height 16px
            margin-right 8px
          .recommend-item
            display inline-block
            border 1px solid rgba(7,17,27,.1)
            border-radius 1px
            font-size 9px
            color rgb(147,153,159)
            line-height 16px
            margin-right 8px
            padding 0 6px
      .rate-time
        position absolute
        right 18px
        top 18px
        font-size 10px
        font-weight 200
        color rgb(147,153,159)
        line-height 12px
</style>
