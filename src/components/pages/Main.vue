<template>
  <div>
    <div class="main-div">
      <keep-alive>
        <router-view/>
      </keep-alive>
    </div>
    <van-tabbar v-model="active" @change="changeTabbar(active)">
      <van-tabbar-item icon="shop">首页</van-tabbar-item>
      <van-tabbar-item icon="records">列表</van-tabbar-item>
      <van-tabbar-item icon="cart">购物车</van-tabbar-item>
      <van-tabbar-item icon="contact" info="20">会员中心</van-tabbar-item>
    </van-tabbar>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        active: 0
      }
    },
    methods: {
      changeTabbar(active) {
        console.log(active)
        switch(active){
          case 0:
            this.$router.push({name:'ShoppingMall'})
            break;
          case 1:
            this.$router.push({name:'CategoryList'})
            break;
          case 2:
            this.$router.push({name:'Cart'})
            break;
          case 3:
            break;
        }
      },
      changeTabBarActive(){
        this.nowPath=this.$route.path
        if(this.nowPath=='/Cart'){
          this.active=2;
        }
      },
    },
    created(){
      this.changeTabBarActive()
    },
    updated(){
      this.changeTabBarActive()
    }
  }
</script>
<style scoped>
.hot-goods{
  height: 130rem;
  overflow: hidden;
  background-color: #fff;
}
</style>