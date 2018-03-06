<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
import vHeader from './components/header/header.vue';

export default {
    created(){
    this.$http.get('/api/seller').then((response) => {
      response = response.body;
      if(response.errno === 0){
        this.seller = response.data;
      }
    })
  },
  components:{
    vHeader,
  },
  data(){
    return{
      seller:{}
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "common/stylus/mixin.styl"

.tab
  display:flex
  width:100%
  height:40px
  line-height:40px
  background #fff
  border-1px-bottom(rgba(7,17,27,.1))
  .tab-item
    flex:1
    text-align:center
    .router-link-active
      color red
</style>
