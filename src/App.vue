<template>
  <div id="app">
    <goods-header :seller='seller'></goods-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to='/goods'>商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to='/ratings'>评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to='/seller'>商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller='seller'></router-view>
    </keep-alive>  
  </div>
</template>

<script>
  import axios from 'axios'
  import goodsHeader from 'components/header/header.vue';
  import goods from 'components/goods/goods.vue';
  const ERR_OK = 'OK';
  const debug = process.env.NODE_ENV !== 'production';
export default {
  name: 'App',
  data () {
    return {
      seller: {
          
      },
      goods:[],
      ratings:{

      }
    }
  },
  mounted () {
    axios.get('/api/data.json').then(this.getData);
  },
  methods: {
    getData(res){
      if(res.statusText === ERR_OK){
        res = res.data
        console.log(res)
        this.seller = res.seller
        this.goods = res.goods
        this.ratings = res.ratings
      }
      
    }
  },
  components: {
    goodsHeader,
    goods
  }
}
</script>

<style  lang='stylus' >
@import "./common/stylus/mixin.styl"
  .tab
    display flex
    width 100%
    height 40px
    line-height 40px
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex 1
      text-align center
      & > a
        display block
        font-size 14px
        color rgb(77, 85, 93)
        &.router-link-active
          color: rgb(240, 20, 20)
</style>
