<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-wrap" @click="showShopList()">
        <div class="icon-shopping_cart" :class="{'icon-active' : totalCount > 0}">
          <div class="shop-num">{{totalCount}}</div>
        </div>
        <div class="price-wrap">
          <span class="price" v-if="totalCount > 0">￥{{totalPrice}}</span>
          <span class="delivery-price">另需配送费￥{{seller.deliveryPrice}}元</span>
        </div>
      </div>
      <div class="shop-btn">
        <div class="shop-less" v-if="totalPrice < seller.minPrice">还差￥{{seller.minPrice}}元起送</div>
        <div class="shop-enough" v-if="totalPrice >= seller.minPrice">购买</div>
      </div>
    </div>
    <transition name="from-bottom">
      <div class="shopcart-list" v-show="isShowShopcart">
        <div class="title-wrap">
          <h2 class="title">购物车</h2>
          <div class="clear-btn" @click="clearList()">清空</div>
        </div>
        <div class="foods-list" ref="foodlist">
          <ul>
            <li class="food-wrap" v-for="food in selectFoods">
              <div class="food-name">{{food.name}}</div>
              <div class="price"><span class="unit">￥</span>{{food.price}}</div>
              <v-cartcontrol :food="food" @add="drop"></v-cartcontrol>
            </li>
          </ul>
        </div>
      </div>
    </transition>
    <transition name="fade">
      <div class="cover" v-if="isShowShopcart" @click="closeCartList()"></div>
    </transition>
    <div class="ball-container">
      <div v-for="ball in balls">
        <transition name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
          <div class="ball" v-show="ball.show">
            <div class="inner inner-hook"></div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import vCartcontrol from '../cartcontrol/cartcontrol.vue'
import BScroll from 'better-scroll';

export default {
  props:{
    seller:{
      type:Object
    },
    selectFoods:{
      type:Array
    }
  },
  components:{
    vCartcontrol
  },
  data(){
    return {
      isShowShopcart:false,
      balls:[
        {
          show:false
        },
        {
          show:false
        },
        {
          show:false
        },
        {
          show:false
        },
        {
          show:false
        },
      ],
      dropBalls:[]
    }
  },
  computed:{
    totalCount(){
      let total = 0;
      this.selectFoods.forEach((food) => {
        total += food.count;
      });
      return total
    },
    totalPrice(){
      let total = 0;
      this.selectFoods.forEach((food) => {
        total += food.price * food.count
      });
      return total
    }
  },
  methods:{
    showShopList(){
      this.isShowShopcart = !this.isShowShopcart;
      this.listRefresh();
    },
    closeCartList(){
      this.isShowShopcart = false;
    },
    listRefresh(){
      this.$nextTick(() => {
        if(!this.foodListScroll){
          this.foodListScroll = new BScroll(this.$refs.foodlist,{
            click:true
          });
        }
        else {
          this.foodListScroll.refresh();
        }
      })
    },
    clearList(){
      this.selectFoods.map((food) => {
        food.count = 0;
      })
    },
    drop(el){
      console.log(el.offsetHeight);
      for(let i = 0;i < this.balls.length;i++){
        let ball = this.balls[i];
        if(!ball.show){
          ball.show = true;
          ball.el = el;
          this.dropBalls.push(ball);
          return;
        }
      }
    },
    beforeDrop(el){
      for(let i = 0;i < this.balls.length;i++){
        let ball = this.balls[i];
        if(ball.show){
          let rect = ball.el.getBoundingClientRect();
          let x = rect.left - 32;
          let y = -(window.innerHeight - rect.top - 22);
          el.style.display = '';
          el.style.transform = `translate(0,${y}px)`;
          el.style.webkitTransform = `translate(0,${y}px)`;
          let inner = el.querySelector('.inner-hook');
          inner.style.transform = `translate(${x}px,0)`;
          inner.style.webkitTransform = `translate(${x}px,0)`
        }
      }
    },
    dropping(el){
      let rf = el.offsetHeight;
      this.$nextTick(() => {
        el.style.transform = `translate(0,0)`;
        el.style.webkitTransform = `translate(0,0)`;
        let inner = el.querySelector('.inner-hook');
        inner.style.transform = `translate(0,0)`;
        inner.style.webkitTransform = `translate(0,0)`
      });

    },
    afterDrop(el){
      let ball = this.dropBalls.shift();
      if(ball){
        ball.show = false;
        el.style.display = 'none';
      }
    }
  }
}
</script>

<style lang="stylus">
.shopcart
  .content
    position absolute
    bottom 0
    left 0
    width 100%
    height 46px
    background #000
    z-index 40
    .content-wrap
      .icon-shopping_cart
        position absolute
        bottom 0
        left 10px
        width 48px
        height 48px
        line-height 48px
        text-align center
        border-radius 50%
        border 6px solid #000
        font-size 24px
        color #fff
        background #666
        &.icon-active
          background rgb(0,160,220)
        .shop-num
          position absolute
          top -2px
          right -10px
          width 28px
          height 16px
          line-height 16px
          font-size 12px
          text-align center
          color #fff
          border-radius 8px
          background red
      .price-wrap
        position absolute
        left 80px
        height 26px
        line-height 26px
        margin 10px 0
        font-size 0
        .price
          font-size 12px
          font-weight 700
          color #fff
          padding-right 10px
          border-right 1px solid #ccc
        .delivery-price
          font-size 12px
          color #ccc
          padding-left 10px
    .shop-btn
      position absolute
      top 0
      right 0
      width 120px
      height 46px
      line-height 46px
      text-align center
      font-size 14px
      font-weight 700
      .shop-less
        width 100%
        height 100%
        color #ccc
        background rgb(43,51,59)
      .shop-enough
        width 100%
        height 100%
        color #fff
        background #3c6
  .shopcart-list
    width 100%
    position absolute
    left 0
    bottom 46px
    z-index 30
    &.from-bottom-enter,&.from-bottom-leave-to
      transform translate(0,100%  )
    &.from-bottom-enter-active,&.from-bottom-leave-active
      transition all .4s
    .title-wrap
      width 100%
      height 40px
      background #f3f5f7
      border-bottom 2px solid rgb(219,222,225)
      overflow hidden
      box-sizing border-box
      padding 0 18px
      .title
        float left
        font-size 14px
        line-height 40px
        font-weight 200
        color rgb(7,17,27)
      .clear-btn
        float right
        font-size 12px
        line-height 40px
        color rgb(0,160,220)
    .foods-list
      width 100%
      max-height 200px
      background #fff
      padding 0 18px 18px
      box-sizing border-box
      overflow hidden
      .food-wrap
        width 100%
        height 48px
        border-bottom 1px solid rgba(7,17,27,.1)
        position relative
        .food-name
          float left
          font-size 14px
          line-height 48px
          color rgb(7,17,27)
        .price
          float right
          font-size 14px
          line-height 48px
          font-weight 700
          color rgb(240,20,20)
          margin-right 94px
          .unit
            font-size 10px
            font-weight normal
        .cartcontrol
          position absolute
          right 0
          top 7px
  .ball-container
    .ball
      position absolute
      left 32px
      bottom 22px
      z-index 100
      transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
      .inner
        width 16px
        height 16px
        border-radius 50%
        background rgb(0,160,220)
        transition: all 0.4s linear
  .cover
    position fixed
    left 0
    top 0
    width 100%
    height 100%
    background rgba(7,17,27,.6)
    backdrop-filter: blur(10px)
    z-index 20
    &.fade-enter-active, &.fade-leave-active
      transition all 0.5s
    &.fade-enter, &.fade-leave-to
      opacity 0
      background rgba(7, 17, 27, 0)
</style>
