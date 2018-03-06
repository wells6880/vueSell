<template>
  <div class="supports-icon">
    <div :class="'supports-icon' + '_' + sizeType + ' ' + maps[supportType] + '_' + sizeType"></div>
  </div>
</template>

<script type="text/ecmascript-6">
export default {
  props:{
    supportType:{
      type:Number
    },
    sizeType:{
      type:Number
    }
  },
  data(){
    return {
      maps:['decrease', 'discount', 'special', 'invoice', 'guarantee'],
    }
  }
}
</script>

<style lang="stylus">
@import "../../common/stylus/mixin.styl";

.supports-icon
  display inline-block
  vertical-align top
  .supports-icon_1
    display inline-block
    width 12px
    height 12px
    background-size cover
    margin-right 4px
    &.decrease_1
      bg-img('decrease_1')
    &.discount_1
      bg-img('discount_1')
    &.special_1
      bg-img('special_1')
    &.invoice_1
      bg-img('invoice_1')
    &.guarantee_1
      bg-img('guarantee_1')
  .supports-icon_2
    display inline-block
    width 16px
    height 16px
    background-size cover
    margin-right 6px
    &.decrease_2
      bg-img('decrease_2')
    &.discount_2
      bg-img('discount_2')
    &.special_2
      bg-img('special_2')
    &.invoice_2
      bg-img('invoice_2')
    &.guarantee_2
      bg-img('guarantee_2')
  .supports-icon_3
    display inline-block
    width 12px
    height 12px
    background-size cover
    margin-right 2px
    &.decrease_3
      bg-img('decrease_3')
    &.discount_3
      bg-img('discount_3')
    &.special_3
      bg-img('special_3')
    &.invoice_3
      bg-img('invoice_3')
    &.guarantee_3
      bg-img('guarantee_3')
  .supports-icon_4
    display inline-block
    width 16px
    height 16px
    background-size cover
    vertical-align top
    margin-right 6px
    &.decrease_4
      bg-img('decrease_4')
    &.discount_4
      bg-img('discount_4')
    &.special_4
      bg-img('special_4')
    &.invoice_4
      bg-img('invoice_4')
    &.guarantee_4
      bg-img('guarantee_4')
</style>
