<template>
  <div class="recommend">
    <Title>推荐歌单</Title>
    <ul class="recommendList">
      <router-link v-for="rem in recommendMusicList" :key="rem.id" to="/" tag="li">
        <div>
          <img :src="rem.picUrl" alt="">
          <span><i class="iconfont icon-erji"></i>  {{rem.playCount | formatNum}}</span>
        </div>
        <p>{{rem.name}}</p>
      </router-link>
    </ul>
    <Title>最新音乐</Title>
    <MusicItem :newMusicList="newMusicList"></MusicItem>
  </div>
</template>

<script>
import Title from '../components/Title'
import MusicItem from '../components/MusicItem'
export default {
  name: 'Recommend',
  components: {
    Title,
    MusicItem
  },
  data(){
    return{
      recommendMusicList:[],
      newMusicList:[],
    }
  },
  beforeRouteEnter(to, from, next){
    next((vm) => {
      vm.$http.get("/personalized?limit=6").then((data) => {
        vm.recommendMusicList = data.data.result;
      });
      vm.$http.get("/personalized/newsong").then((data) => {
        vm.newMusicList = data.data.result;
        console.log(data.data.result);
      });
    });
  },
  filters:{
    formatNum(value){
      return (value / 10000).toFixed(1) + "万";
    }
  }
}
</script>


<style lang="less" scoped>
ul.recommendList {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  li {
    width: 33%;
    margin-bottom: 12px;
    div {
      position: relative;
      span {
        top: 2px;
        right: 3px;
        position: absolute;
        color: white;
        font-size: 12px;
        text-shadow: 1px 1px 3px rgba(0,0,0,0.5);
        i{
          font-size: 11px;
        }
      }

    }
    p{
      font-size: 12px;
      text-align: left;
      word-wrap: none;
      padding: 0px 8px;
    }
  }
}
</style>
