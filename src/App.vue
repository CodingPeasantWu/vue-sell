<template>
  <div id="app">
    <goods-header :seller='seller'></goods-header>
    <keep-alive>
      <router-view></router-view>
    </keep-alive>  
  </div>
</template>

<script>
  import axios from 'axios'
  import goodsHeader from 'components/header/header.vue';

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
    axios.get('/api/data.json').then(this.getData)
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
    goodsHeader
  }
}
</script>

<style>

</style>
