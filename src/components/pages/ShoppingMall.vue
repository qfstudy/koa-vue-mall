<template>
  <div>
    <div class="search-bar">
      <van-row>
        <van-col span="3">
          <img :src="locationIcon" width="80%" class="location-icon" />
        </van-col>
        <van-col span="16">
          <input type="text" class="search-input">
        </van-col>
        <van-col span="5">
          <van-button size="mini">
            查找
          </van-button>
        </van-col>
      </van-row>
    </div>
    <!--swiper area-->
    <div class="swiper-area">
      <van-swipe :autoplay="1000">
        <van-swipe-item v-for="( banner ,index) in bannerPicArray" :key="index">
          <img v-lazy="banner.image" width="100%" />
        </van-swipe-item>
      </van-swipe>
    </div>
    <!-- type bar -->
    <div class="type-bar">
      <div v-for="(cate,index) in category" :key="index">
        <img v-lazy="cate.image">
        <span>{{cate.mallCategoryName}}</span>
      </div>
    </div>
    <!-- adbanner -->
    <div>
      <img v-lazy="adBarner" width="100%">
    </div>
    <!-- recommend -->
    <div class="recommend-area">
      <div class="recommend-title">
        商品推荐
      </div>
      <div class="recommend-body">
        <swiper :options="swiperOption">
          <swiper-slide v-for="(item,index) in recommentGoods" :key="index">
            <div class="recommend-item">
              <img :src="item.image" alt="" width="80%">
              <div>{{item.goodsName}}</div>
              <div>￥{{item.price | moneyFilter}}(￥{{item.mallPrice | moneyFilter}})</div>
            </div>
          </swiper-slide>
        </swiper>
      </div>
    </div>
    <floor-component :floorData="floor1" :floorTitle="floorName.floor1"></floor-component>
    <floor-component :floorData="floor2" :floorTitle="floorName.floor2"></floor-component>
    <floor-component :floorData="floor3" :floorTitle="floorName.floor3"></floor-component>
    <!--Hot Area-->
    <div class="hot-area">
        <div class="hot-title">热卖商品</div>
        <div class="hot-goods">
          <!--这里需要一个list组件-->
          <van-list>
            <van-row gutter="20">
                <van-col span="12" v-for="( item, index) in hotGoods" :key="index">
                    <goods-info :goodsId="item.goodsId" :goodsImage="item.image" :goodsName="item.name" :goodsPrice="item.price"></goods-info>
                </van-col>
            </van-row>
          </van-list>
        </div>
    </div>
  </div>
</template>

<script>
  import {swiper,swiperSlide} from 'vue-awesome-swiper'
  import 'swiper/dist/css/swiper.css'
  import axios from 'axios'
  import floorComponent from '../component/floorComponent'
  import {toMoney} from '@/filter/moneyFilter.js'
  import goodsInfo from '../component/goodsInfoComponent'
  import url from '@/serviceAPI.config.js'

  export default {
    data() {
      return {
        swiperOption: {
          slidesPerView: 3
        },
        locationIcon: require('../../assets/images/location.png'),
        bannerPicArray: [],
        category: [],
        adBarner: '',
        recommentGoods: [],
        floor1: [],
        floor2:[],         //楼层1的数据
        floor3:[],         //楼层1的数据
        floorName:{},
        hotGoods:[] //热卖商品
      }
    },
    filters:{
      moneyFilter(money){
        return toMoney(money)
      }  
    },
    components: {
      swiper,
      swiperSlide,
      floorComponent,
      goodsInfo
    },
    created() {
      axios({
          url: url.getShoppingMallInfo,
          method: 'get',
        })
        .then(response => {
          // console.log(response)
          if (response.status === 200) {
            this.category = response.data.data.category
            this.adBarner = response.data.data.advertesPicture.PICTURE_ADDRESS
            this.bannerPicArray = response.data.data.slides
            this.recommentGoods = response.data.data.recommend
            this.floor1 = response.data.data.floor1
            this.floor2 = response.data.data.floor2              //楼层2数据
            this.floor3 = response.data.data.floor3              //楼层3数据
            this.floorName = response.data.data.floorName 
            this.hotGoods = response.data.data.hotGoods 
          }
        })
        .catch(error => {
          console.log(error)
        })
    }
  }

</script>

<style scoped>
  .search-bar {
    height: 2.2rem;
    background-color: #e5017d;
    line-height: 2.2rem;
    overflow: hidden;
  }

  .search-input {
    width: 100%;
    height: 1.3rem;
    border-top: 0;
    border-left: 0;
    border-right: 0;
    border-bottom: 1px solid #fff !important;
    background-color: #e5017d;
    color: white;
  }

  .location-icon {
    padding-top: .2rem;
    padding-left: .3rem;
  }

  .swiper-area::after {
    content: '';
    clear: both;
    display: block;
  }

  .swiper-area,
  .swiper-area img {
    max-height: 8rem;
  }

  .type-bar {
    background-color: white;
    display: flex;
    justify-content: space-between;
    flex-direction: row;
    flex-wrap: nowrap;
  }

  .type-bar img {
    width: 90%;
    max-width: 4rem;
    max-height: 4rem;
  }

  .type-bar {
    padding: .3rem;
    font-size: 12px;
    text-align: center;
    flex: 1;
  }

  .recommend-area {
    background-color: #fff;
    margin-top: .3rem;
  }

  .recommend-title {
    border-bottom: 1px solid #eee;
    font-size: 14px;
    padding: .2rem;
    color: #e5017d;
  }

  .recommend-body {
    border-bottom: 1px solid #eee;
  }

  .recommend-item {
    width: 99%;
    border-right: 1px solid #eee;
    font-size: 12px;
    text-align: center;
  }

  .hot-area{
    text-align: center;
    font-size:14px;
    height: 1.8rem;
    line-height:1.8rem;
  }
  .hot-goods{
    height: 130rem;
    overflow: hidden;
    background: #fff;
  }
</style>
