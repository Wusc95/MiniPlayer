<template>
  <div class="song_wrapper" ref="songs">
    <ul class="song_list">
      <li v-for="(item, index) in list" :key="index" @click="playMusic(item.id)">{{item.name}}</li>
    </ul>
  </div>
</template>

<script>
//axios包
import axios from "axios";
//iscroll包
import iscroll from "iscroll";
export default {
  data() {
    return {
      list: [],
      scroll:'',
      playing:false,
      comments:''
    };
  },
  methods: {
      playMusic(id){
        //获取音乐播放地址
        axios({
          url:'https://autumnfish.cn/song/url?id='+id
        }).then(res=>{
          this.$parent.musicUrl=res.data.data[0].url;
        });
        //获取音乐的评论数据
        axios({
          url:"https://autumnfish.cn/comment/music?id="+id
        }).then(res=>{
          this.comments=res.data.comments;
          window.console.log(this.comments);
          this.$parent.getComments();
        });

      }
  },
  mounted() {
    this.scroll = new iscroll(this.$refs.songs, {
      mouseWheel: true,
      scrollbars: true
    });
  }
};
</script>

<style>
</style>