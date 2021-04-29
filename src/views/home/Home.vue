<template>
  <div id="home">
    <nav-bar class="home-nav"><div  slot="center">购物街</div></nav-bar>
     
     <home-swiper :banners="banners" ></home-swiper>
      <home-recommends :recommends="recommends"></home-recommends>
      <this-week></this-week>
      <tab-control :titles="['流行','新款','精选']"  
      @tabClick="tabClick" 
      ></tab-control>
      <good-list :goods="showGoods" style="background-color: white;"></good-list>


  </div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import {getHomeMultidata} from 'network/home'
import {getHomeGoods} from 'network/home'
import HomeSwiper from './childComps/HomeSwiper'
import HomeRecommends from './childComps/HomeRecommends'
import ThisWeek from './childComps/ThisWeek'
import TabControl from '../../components/content/tabControl/TabControl'
import GoodList from '../../components/content/goods/GoodsList'
export default {
  name:'Home',
  components:{
     NavBar,
     HomeSwiper,
     HomeRecommends,
     ThisWeek,
     TabControl,
     GoodList
  },
  data() {
    return {
      result:null,
      banners:[],
      recommends:[],
      tabOffseTop:0,
      recommends:[],
      goods:{
        'pop':{page:0,list:[]},
        'new':{page:0,list:[]},
        'sell':{page:0,list:[]},

      },
      currentType:'pop'
    }
  },
  // 声明周期函数  首页创建完成之后 直接触发
  created() {
       this.getHomeMultidata()

       this.getHomeGoods('pop')
       this.getHomeGoods('new')
       this.getHomeGoods('sell')
  },
  mounted() {
   


  },
  methods: {
     tabClick(index){   
      // console.log(index)
      switch(index){
        case 0:
          this.currentType = 'pop'
         console.log(this.currentType = 'pop')
          break
        case 1:
          this.currentType = 'news'
           console.log(this.currentType = 'new')
          break
        case 2:
          this.currentType = 'sell'
          console.log(this.currentType = 'sell')
          break
      }
     },
   /** 
    * 网络请求相关方法
    */
    getHomeMultidata(){
      getHomeMultidata().then(res => {
      // 将端口数据存入 data中的result中
      this.result = res
      this.banners = res.data.data.banner.list;
      this.recommends = res.data.data.recommend.list;
    }) 
    },


    getHomeGoods(type){
      const page = this.goods[type].page + 1
  
      getHomeGoods(type,page).then(res => {
       
      this.goods[type].list.push(...res.data.data.list)
      this.goods[type].page +=1
    })
    },
},
    computed: {
    showGoods(){
        console.log("aaa",this.goods[this.currentType])
      return this.goods[this.currentType].list
    }
  },
}
</script>



<style scoped>
/* scoped作用域 */
.home-nav{
  background-color: var(--color-tint);
  color: white;

  /* position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9; */

}
#home{
  height: 100vh;
}
.tab-control{
  /* 粘贴定位 */
  position: sticky;
  top:44px;
  background-color: #fff;
}
.content{
  height: calc(100% - 93px);
  /* margin-top: 44px; */
  /* height:100%; */
  overflow: hidden;
}
.tabControlwk{
  position:fixed;
  z-index: 9;
  right: 0;
  left: 0;
  background-color: white;
}
</style>