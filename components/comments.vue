<template>
  <div class="comment_list" ref="comments">
    <div v-for="(item, index) in comments" :key="index">
      <dl>
        <dt><img :src="item.user.avatarUrl" alt /></dt>
        <dd class="name">{{item.user.nickname}}</dd>
        <dd class="detail">{{item.content}}</dd>
      </dl>
    </div>
  </div>
</template>

<script>
//iscroll包
// import iscroll from "iscroll";
export default {
  data() {
    return {
      comments: "",
      scroll:''
    };
  },
  methods: {
    
  },
  mounted() {
    // this.scroll = new iscroll(this.$refs.comments, {
    //   mouseWheel: true,
    //   scrollbars: true
    // });
  },
};
</script>

<style>
</style>