<template>
  <div class="row">
    <h2 v-if="firstView">输入用户名搜索</h2>
    <h2 v-if="loading">LOADING...</h2>
    <h2>{{erroMsg}}</h2>
    <div class="card" v-for="(user,index) in users" :key="index">
      <a :href="user.url">
        <img :src="user.avatar" style="width: 100px" />
      </a>
      <p class="card-text">{{user.name}}</p>
    </div>
  </div>
</template>

<script>
import PubSub from "pubsub-js";
import axios from "axios";

export default {
  data() {
    return {
      firstView: true,
      loading: false,
      users: null,
      erroMsg: ""
    };
  },
  mounted() {
    //订阅搜索的消息
    PubSub.subscribe("search", (msg, searchName) => {
      const url = `https://api.github.com/search/users?q=${searchName}`;
      //更新状态(请求中)
      this.firstView = false;
      this.loading = true;
      this.users = null;
      this.erroMsg = "";
      //发送ajax请求
      axios.get(url).then(response => {
        const result = response.data;
        const users = result.items.map(item => ({
          url: item.html_url,
          avatar: item.avatar_url,
          name: item.login
        }));
        //成功更新状态
        this.loading = false;
        this.users = users
        //失败更新状态
      }).catch(error => {
        this.loading = false
        this.erroMsg = '请求失败'
      })
    });
  }
};
</script>

<style>
.card {
  float: left;
  width: 33.333%;
  padding: 0.75rem;
  margin-bottom: 2rem;
  border: 1px solid #efefef;
  text-align: center;
}

.card > img {
  margin-bottom: 0.75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}
</style>