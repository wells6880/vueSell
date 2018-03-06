<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="decrease" v-if="this.food.count > 0">
        <i class="inner icon-remove_circle_outline" @click.stop.prevent="foodNumDecrease()"></i>
      </div>
    </transition>
    <span class="count" v-if="this.food.count > 0">{{food.count}}</span>
    <i class="inner icon-add_circle" @click.stop="foodNumAdd"></i>
  </div>
</template>

<script type="text/ecmascript-6">
import Vue from 'vue'
export default {
  props:{
    food:{
      type:Object
    }
  },
  methods:{
    foodNumAdd(event){
      if(!this.food.count){
        Vue.set(this.food,'count',1)
      }
      else {
        this.food.count++
      }
      this.$emit('add',event.target)
    },
    foodNumDecrease(){
      if(this.food){
        this.food.count--
      }
    }
  }
}
</script>

<style lang="stylus">
.cartcontrol
  font-size 0
  .decrease
    display inline-block
    &.move-enter-active,&.move-leave-active
      transition all .4s linear
    &.move-enter,&.move-leave-to
      transform translate(24px,0)
      opacity 0
      .inner
        transition all .4s linear
        transform rotate(90deg)
  .inner
    display inline-block
    color rgb(0,160,220)
    font-size 24px
    line-height 24px
    padding 5px
    transition all .4s linear
  .count
    display inline-block
    width 14px
    font-size 10px
    color rgb(147,153,159)
    line-height 24px
    text-align center
    padding 5px 0
    vertical-align top
</style>
