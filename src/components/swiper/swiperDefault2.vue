<template>
  <div>
    <swiper :options="swiperOption">
      <swiper-slide class="swiper-slide" v-for="(item,index) in slide" :key="index">
        slide{{item}}
      </swiper-slide>
      <div class="swiper-pagination" slot="pagination"></div>
    </swiper>
  </div>
</template>

<script>
  import {swiper,swiperSlide} from 'vue-awesome-swiper'
  import 'swiper/dist/css/swiper.css'
  export default {
    data() {
      return {
        slide: [1,2,3,4,5,6],
        swiperOption:{
          pagination:{
            el:'.swiper-pagination'
          }
        }
      }
    },
    components:{
      swiper,
      swiperSlide
    }
  }
</script>

<style scoped>
  .swiper-slide{
    height: 4rem;
    text-align: center;
    padding-top: 3rem;
    border-bottom: 1px solid #ccc; 
  }
</style>