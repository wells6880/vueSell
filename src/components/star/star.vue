<template>
  <div class="stars" :class="starSize">
    <span class="star" v-for="starType in getStarType" :class="starType"></span>
  </div>
</template>

<script type="text/ecmascript-6">

let starOn='on',
    starHalf='half',
    starOff='off',
    Length=5;

export default {
  props: {
    score: {
      type: Number
    },
    size:{
      type:Number
    }
  },
  computed: {
    starSize(){
      return 'stars-' + this.size;
    },
    getStarType() {
      let starTypes = [];
      for (let i = 0; i < Math.floor(this.score); i++) {
        starTypes.push(starOn);
      }
      if (this.score !== Math.floor(this.score)) {
        starTypes.push(starHalf);
      }
      if (starTypes.length < Length) {
        for (let i = 0; i < (Length - starTypes.length); i++) {
          starTypes.push(starOff);
        }
      }
      return starTypes;
    },
  }
};
</script>

<style lang="stylus">
@import "../../common/stylus/mixin.styl"

.stars
  font-size 0
  text-align center
  &.stars-48
    .star
      display inline-block
      width 20px
      height 20px
      margin-right 22px
      background-size cover
      background-repeat no-repeat
      &:last-child
        margin-right 0
      &.on
        bg-img('star48_on')
      &.half
        bg-img('star48_half')
      &.off
        bg-img('star48_off')
  &.stars-36
    .star
      display inline-block
      width 15px
      height 15px
      margin-right 6px
      background-size cover
      background-repeat no-repeat
      &:last-child
        margin-right 0
      &.on
        bg-img('star36_on')
      &.half
        bg-img('star36_half')
      &.off
        bg-img('star36_off')
  &.stars-24
    .star
      display inline-block
      width 10px
      height 10px
      margin-right 3px
      background-size cover
      background-repeat no-repeat
      &:last-child
        margin-right 0
      &.on
        bg-img('star24_on')
      &.half
        bg-img('star24_half')
      &.off
        bg-img('star24_off')
</style>
