<template>
  <div class="wrap">
    <div class="play_wrap" id="player">
      <div class="search_bar">
        <img src="./images/player_title.png" />
        <input type="text" v-model="searchMusic" @keyup.enter="search" />
      </div>
      <div class="center_con">
        <!-- 歌单列表 -->
        <!-- <div class="song_wrapper">
          <ul class="song_list">
            <li>岁月神偷--周笔畅</li>
            <li>岁月神偷--金玟岐</li>
            <li>岁月神偷--汪苏泷</li>
            <li>岁月神偷--不才</li>
            <li>岁月神偷（磁性男声版）（Cover：金玟岐）--于大海（于黑蛋）</li>
          </ul>
        </div>-->
        <musicList ref="musicList"></musicList>

        <!-- 中心动画 -->
        <!-- <div class="player_con">
          <img src="images/player_bar.png" class="play_bar playing" />
          
          <img src="images/disc.png" class="disc autoRotate playing" />
          <img src="./images/cover.png" class="cover autoRotate playing" />
        </div>-->
        <center ref="center"></center>

        <!-- 评论列表-->
        <!-- <div class="comment_list">
          <div>
            <dl>
              <dt>
                <img
                  src="https://p3.music.126.net/2cwPUmMgb6XImVwNy-FcIw==/109951163863553426.jpg"
                  alt
                />
              </dt>
              <dd class="name">Teoeo_</dd>
              <dd
                class="detail"
              >你没听出来汪老师一直在配合刘乐瑶唱吗，他大部分时间在给刘乐瑶和音，而且唱的是女生的key，因为是孩子们的节目，他想让孩子更多的展现自己的优点，所以汪老师尽量地配合刘乐瑶的声音唱，如果是汪苏泷一个人唱，他不会这样唱的。</dd>
            </dl>
          </div>
        </div>-->
        <comments ref="comments"></comments>
      </div>
      <div class="audio_con">
        <audio :src="musicUrl" controls autoplay loop class="myaudio" @pause="pause" @play="play"></audio>
      </div>
    </div>
  </div>
</template>

<script>
import musicList from "./components/musicList.vue";
import center from "./components/center.vue";
import comments from "./components/comments.vue";
//axios包
import axios from "axios";
export default {
  data() {
    return {
      searchMusic: "神话",
      musicUrl: ""
    };
  },
  methods: {
    search() {
      if (this.searchMusic != "") {
        axios({
          url:
            "https://autumnfish.cn/search?keywords=" +
            this.searchMusic +
            "&_t=" +
            Math.random() * 99999
        }).then(res => {
          // window.console.log(res);
          this.$refs.musicList.list = res.data.result.songs;
          window.console.log(this.$refs.musicList.list);
          this.$nextTick(() => {
            this.$refs.musicList.scroll.refresh();
          });
        });
      } else {
        alert("输入歌曲名查询");
      }
    },
    pause() {
      this.$refs.center.playing = false;
    },
    play() {
      this.$refs.center.playing = true;
    },
    getComments(){
      this.$refs.comments.comments = this.$refs.musicList.comments;
    }
  },
  components: {
    musicList,
    center,
    comments
  }
};
</script>

<style>
body,
ul,
dl,
dd {
  margin: 0px;
  padding: 0px;
}

.wrap {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: url("images/bg.jpg") no-repeat;
  background-size: 100% 100%;
}

.play_wrap {
  width: 800px;
  height: 544px;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -400px;
  margin-top: -272px;
  /* background-color: #f9f9f9; */
}

.search_bar {
  height: 60px;
  background-color: #1eacda;
  overflow: hidden;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  z-index: 11;
}

.search_bar img {
  margin-left: 23px;
}

.search_bar input {
  margin-right: 23px;
  width: 296px;
  height: 34px;
  border-radius: 17px;
  border: 0px;
  background: url("images/zoom.png") 265px center no-repeat
    rgba(255, 255, 255, 0.45);
  text-indent: 15px;
  outline: none;
}

.center_con {
  height: 435px;
  background-color: rgba(255, 255, 255, 0.5);
  display: flex;
}

.song_wrapper {
  width: 200px;
  height: 435px;
  box-sizing: border-box;
  padding: 10px;
  list-style: none;
  background: url("images/line.png") right center no-repeat;
  position: relative;
  overflow: hidden;
}

.song_list li {
  font-size: 12px;
  color: #333;
  line-height: 36px;
  width: 180px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  cursor: pointer;
}

.song_list .active {
  color: #da651e;
}

.player_con {
  width: 400px;
  height: 435px;
  position: relative;
}

.disc {
  position: absolute;
  left: 73px;
  top: 60px;
  z-index: 9;
}

.cover {
  position: absolute;
  left: 125px;
  top: 112px;
  width: 150px;
  height: 150px;
  border-radius: 75px;
  z-index: 8;
}

.comment_list {
  width: 200px;
  height: 435px;
  box-sizing: border-box;
  padding: 10px;
  list-style: none;
  background: url("images/line.png") left center no-repeat;
  overflow: hidden;
  position: relative;
  overflow: auto;
}

.comment_list dl {
  padding-left: 55px;
  position: relative;
  margin-bottom: 20px;
}

.comment_list dt {
  position: absolute;
  left: 4px;
  top: 0px;
}

.comment_list dt img {
  width: 40px;
  height: 40px;
  border-radius: 20px;
}

.comment_list dd {
  font-size: 12px;
}

.comment_list .name {
  font-weight: bold;
  color: #333;
  margin-top: 5px;
}

.comment_list .detail {
  color: #666;
  margin-top: 5px;
  line-height: 18px;
}

.audio_con {
  height: 50px;
  background-color: #f1f3f4;
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}

.myaudio {
  width: 800px;
  height: 40px;
  margin-top: 5px;
  outline: none;
  background-color: #f1f3f4;
}

/* 旋转的动画 */
@keyframes Rotate {
  from {
    transform: rotateZ(0);
  }

  to {
    transform: rotateZ(360deg);
  }
}

/* 旋转的类名 */
.autoRotate {
  animation-name: Rotate;
  animation-iteration-count: infinite;
  animation-play-state: paused;
  animation-timing-function: linear;
  animation-duration: 5s;
}

/* 是否正在播放 */
.playing {
  animation-play-state: running;
}

.play_bar {
  position: absolute;
  left: 200px;
  top: -10px;
  z-index: 10;
  transform: rotate(-25deg);
  transform-origin: 12px 12px;
  transition: 1s;
}

/* 播放杆 转回去 */
.play_bar.playing {
  transform: rotate(0);
}
</style>