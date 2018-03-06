<template>
  <div>
    <div class="goods">
      <div class="goods-menu" ref="menu">
        <ul>
          <li v-for="(item,index) in goods" class="goods-menu-name" @click="selectMenu(index,$event)" :class="{'menu-active':activeIndex === index}" ref="menuList">
            <span class="goods-menu-name-text">
              <v-supportsicon :supportType="item.type" :sizeType="3" v-if="item.type > 0"></v-supportsicon>
              {{item.name}}
            </span>
          </li>
        </ul>
      </div>
      <div class="goods-foods" ref="foods">
        <ul>
          <li v-for="(item,index) in goods" class="foods-type" ref="foodsType">
            <div class="foods-type-wrap">
              <div class="foods-type-border"></div>
              <h2 class="foods-type-title">
                {{item.name}}
              </h2>
            </div>
            <ul class="foods-list">
              <li v-for="food in item.foods" class="food-wrap" @click="selectFood(food)">
                <div class="food-img">
                  <img :src="food.icon">
                </div>
                <div class="food-content">
                  <div class="food-name">{{food.name}}</div>
                  <div class="food-description">
                    {{food.description}}
                  </div>
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
                </div>
                <v-cartcontrol :food="food" @add="addFood"></v-cartcontrol>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
    <v-shopcart :seller="seller" :selectFoods="selectFoods" ref="shopcart"></v-shopcart>
    <v-food :food="selectedFood" ref="food" @add="addFood"></v-food>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll';
import vCartcontrol from '../cartcontrol/cartcontrol.vue'
import vShopcart from '../shopcart/shopcart.vue';
import vFood from '../food/food.vue'
import vSupportsicon from '../supportsicon/supportsicon.vue'

export default{
  components:{
    vCartcontrol,
    vShopcart,
    vFood,
    vSupportsicon
  },
  created(){
    this.$http.get('/api/goods').then((response) => {
      response = response.body;
      if(response.errno === 0){
        this.goods = response.data;
        this.$nextTick(() => {
          this._initScoll();
          this._clacHeight();
        })
      }
    })
  },
  props:{
    seller:{
      type:Object
    }
  },
  data(){
    return{
      goods:[],
      foodsHeight:[],
      scrollY:0,
      shop:{},
      selectedFood:{},
    }
  },
  computed:{
    activeIndex(){
      for(let i=0;i < this.foodsHeight.length;i++){
        let height1 = this.foodsHeight[i];
        let height2 = this.foodsHeight[i + 1];
        if(!height2 || this.scrollY >= height1 && this.scrollY < height2){
          this.menuScroll.scrollToElement(this.$refs.menuList[i],300,0,-100)
          return i
        }
      }
      return 0
    },
    selectFoods(){
      let foods = [];
      this.goods.forEach((type) => {
        type.foods.forEach((food) => {
          if(food.count){
            foods.push(food);
          }
        })
      })
      return foods
    }
  },
  methods:{
    selectMenu(idx,event){
//      if (!event._constructed) {
//        return;
//      }
      let foodsType=this.$refs.foodsType
      this.foodsScroll.scrollToElement(foodsType[idx],300);
    },
    _initScoll(){
      this.menuScroll=new BScroll(this.$refs.menu,{
        click:true
      })
      this.foodsScroll=new BScroll(this.$refs.foods,{
        click:true,
        probeType:3
      })
      this.foodsScroll.on('scroll', (pos) => {
        if (pos.y <= 0) {
          this.scrollY = Math.abs(Math.round(pos.y));
        }
        else{
          this.scrollY = 0;
        }
      });
    },
    _clacHeight(){
      let foodsType = this.$refs.foodsType;
      let height = 0;
      this.foodsHeight.push(height);
      for(let i=0;i < foodsType.length;i++){
        height += foodsType[i].clientHeight
        this.foodsHeight.push(height);
      }
    },
    selectFood(food){
      this.selectedFood = food;
      this.$refs.food.show();
    },
    addFood(target){
      this._drop(target)
    },
    _drop(target){
      //异步加载，优化动画
      this.$nextTick(() => {
        this.$refs.shopcart.drop(target)
      })
    },
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"

.goods
  width 100%
  display flex
  overflow hidden
  position: absolute
  top: 168px
  bottom: 46px
  left 0
  .goods-menu
    flex 0 0 80px
    background #f3f5f7
    .goods-menu-name
      display table
      height 54px
      width 56px
      padding 0 12px
      font-size 0
      .goods-menu-name-text
        display: table-cell
        vertical-align: middle
        font-size: 12px
        line-height 14px
        border-1px-bottom(rgba(7,17,27,.1))
      &.menu-active
        position relative
        top -1px
        height 55px
        background #fff
        .goods-menu-name-text
          border-1px-bottom(#fff)
  .goods-foods
    flex 1
    .foods-type
      position relative
      .foods-type-wrap
        .foods-type-border
          width 5px
          height 26px
          background #d9dde1
          position absolute
          left 0
          top 0
        .foods-type-title
          height 26px
          line-height 26px
          padding-left 15px
          font-size 12px
          font-weight 600
          color rgb(147,153,159)
          background #f3f5f7
      .foods-list
        .food-wrap
          padding 18px 0
          margin 0 18px
          overflow hidden
          border-1px-bottom(rgba(7,17,27,.1))
          display flex
          position relative
          &:last-child
            border-bottom none
          .food-img
            flex 0 0 57px
            img
              width 57px
              height 57px
          .food-content
            flex 1
            margin-left 10px
            .food-name
              font-size 14px
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
          .cartcontrol
            position absolute
            right 18px
            bottom 12px
</style>
