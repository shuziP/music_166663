<template>
  <div class="home">

    <nav class="flex">
      <main class="warp flex flex-align-items-center">
        <aside class="flex">
      <img alt="163 logo" src="../assets/logo.png">
      <section class="logo">
        <img src="logoImg" alt="">

      </section>
      <ul class="flex flex-align-items-center">
        <li v-for="(item,index) of navArr">
          {{item.name}}
        </li>
      </ul>


        </aside>
<!--交互-->

          <section class="flex flex-align-items-center">

            <input type="text"  @keyup.enter="search" v-model ="query">

            <input type="button" value="创作者中心">
            <input type="button" value="登录">

          </section>



      </main>
    </nav>

<!--    songs start-->
    <section>
      <ul>
        <li v-for="(item,home) in songsArr" :key="item.album.id" class="flex" @click="playMusic(item.id)">
          <div>{{home+1}}</div>
          <div>{{item.album.name}}</div>
          <div>{{item.artists[0].name}}</div>


        </li>
      </ul>
    </section>
<!--    songs end-->

<!--    播放按钮-->
    <audio :src="playUrl" autoplay controls></audio>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios';

export default {
  name: 'Home',
  data(){
    return {
      // es6模块化最终打包成commonjs
      logoImg:require("@/assets/logo.png"),
      navArr:[
        {'name':'发现阴乐'},
        {'name':'我的阴乐'},
        {'name':'朋友'},
        {'name':'商场'},
        {'name':'阴乐人'},
        {'name':'下载客户端hot'},

      ],
      query:'',
      songsArr:[],
      playUrl:'',
    }
  },

 methods:{
    search() {
      console.log(this.query);
      axios.get(`http://10.23.117.44:3000/search?keywords=${this.query}`).then(res=>{
        console.log(res.data.result.songs)
        this.songsArr = res.data.result.songs
      })
    },
   playMusic(id){
      axios.get(`http://10.23.117.44:3000/song/url?id=${id}`).then(res=>{
        console.log(res.data.data[0]);
        this.playUrl = res.data.data[0].url;
      })
   }
 }

}
</script>
<style scoped lang="less">
nav{
  height: 70px;
  box-sizing: border-box;
  background: #242424;
  border-bottom: 1px solid #000;
  .logo{
    img{
      width: 157px;
    }
  }
  ul{
    li{
      color: #ccc;
      font-size: 14px;
      margin: 0 10px 0 20px;
    }
  }

}


</style>