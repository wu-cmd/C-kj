<template>
  <div id="home">
    <nav-bar class="home-nav"><div  slot="center">购物街</div></nav-bar>
  </div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
export default {
  name:'Home',
  components:{
     NavBar
  },
  data() {
    return {
    
    }
  },
  // 声明周期函数  首页创建完成之后 直接触发
  created() {
   getHomeMultidata().then(res=>{
     console.log(res)
   })
  },
  mounted() {
   


  },
  methods: {
  
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
</style>