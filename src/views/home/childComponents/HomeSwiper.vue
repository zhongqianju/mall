<template>
  <van-swipe :autoplay="3000" indicator-color="white" class="vanSwiper">
    <van-swipe-item
      v-for="(item,i) in banners"
      :key="i"
      @touchstart="touchStart"
      @touchmove="touchMove"
      @touchend="touchEnd">
        <div  :@click="test(item.link)" >
          <img :src="item.image" alt="" @load="imageLoad">
        </div>
    </van-swipe-item>
  </van-swipe>
</template>

<script>
  import Vue from 'vue';
  import { Swipe, SwipeItem } from 'vant';
  import 'vant/lib/index.css';

  Vue.use(Swipe).use(SwipeItem);

  export default {
    name: "MainSwiper",
    props:{
      banners:Array,
      default () {
        return []
      }
    },
    methods :{
      imageLoad(){
        this.$emit('imageLoad')
      },
      /**
       * 拖动事件的处理
       */
      touchStart: function (e) {
        this.falg = false;
      },

      touchMove: function (e) {
        this.falg = true;
      },

      touchEnd: function (e) {
        if( !this.falg ) {
          window.location.href=this.link;
        }
      },
      test:function (url) {
        this.link=url;
      }
    },
    data(){
      return{
        link:null
      }
    }
  }
</script>

<style scoped>
  img {
    width: 100%;
  }
</style>
