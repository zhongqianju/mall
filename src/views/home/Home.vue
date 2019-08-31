<template>
  <div id="home">
    <tab-bar>
      <slot slot="navBarCenter">购物街</slot>
    </tab-bar>
    <home-swiper :banners="banners"></home-swiper>
    <home-recommend-information :recommends="recommends"></home-recommend-information>
    <home-feature></home-feature>
    <tab-control :title="['流行','新款','精选']" @getTabControlIndex="itemClick"></tab-control>
    <goods-list :goodslist="goods[currentTabType].list"></goods-list>
    111
  </div>

</template>

<script>
  import TabBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabcontrol/TabControl'
  import GoodsList from 'components/content/goods/GoodsList'

  import HomeSwiper from './childComponents/HomeSwiper'
  import HomeRecommendInformation from './childComponents/RecommendInformation'
  import HomeFeature from './childComponents/HomeFeature'



  import {getHomeMultidata,getGoodsData} from 'network/home'

  export default {
    name: "Home",
    data() {
      return {
        banners:[],
        recommends:[],
        currentTabType:'pop',
        goods:{
          pop:{page:0,list:[]},
          new:{page:0,list:[]},
          sell:{page:0,list:[]}
        }
      }
    },
    components:{
      TabBar,
      HomeSwiper,
      HomeRecommendInformation,
      HomeFeature,
      TabControl,
      GoodsList
    },
    created() {
      this.getHomeMultidata()
      this.getGoodsData('pop',1)
      this.getGoodsData('new',1)
      this.getGoodsData('sell',1)
    },
    methods:{
      /*
      * 网络请求的方法
      * */
      getHomeMultidata(){
        getHomeMultidata().then(res => {
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getGoodsData(type,page) {
        getGoodsData(type,page).then(res => {
          this.goods[type].list = res.data.list;
        })
      },

      /*
      * 事件监听的方法
      * */
      itemClick(i) {
        switch (i) {
          case 0 :{
            this.currentTabType = 'pop';
            break;
          }
          case 1 :{
            this.currentTabType = 'new';
            break;
          }
          case 2 :{
            this.currentTabType = 'sell';
            break;
          }
        }

      }

    }
  }
</script>

<style scoped>
  #home {
    padding-top: 44px;
    padding-bottom: 49px;
  }

</style>
