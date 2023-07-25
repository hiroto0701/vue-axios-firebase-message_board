<template>
  <div>
    <h1>掲示板</h1>
    名前
    <div>
      <input type="text" v-model="name">
    </div>
    コメント
    <div>
      <textarea v-model="comment" cols="30" rows="10"></textarea>
      <br>
      <br>
      <button @click="submitPosts">投稿する</button>
      <br>
      <br>
      <h2>投稿一覧</h2>
      <div v-for="post in posts" :key="post.name">
        <hr>
        <p>名前：{{ post.fields.name.stringValue }}</p>
        <p>コメント：{{ post.fields.comment.stringValue }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      name: '',
      comment: '',
      posts: ''
    }
  },
  methods: {
    submitPosts() {
      axios.post(
        "https://firestore.googleapis.com/v1/projects/vue-test-63736/databases/(default)/documents/posts",
        {
          fields : {
            name: {
              stringValue: this.name
            },
            comment: {
              stringValue: this.comment
            }
          }
        }
      ).then(() => {
        this.name = '';
        this.comment= '';
        this.getPosts();
      });
    },
    getPosts() {
      axios.get(
        "https://firestore.googleapis.com/v1/projects/vue-test-63736/databases/(default)/documents/posts",
      ).then(res => {
        this.posts = res.data.documents;
      });
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
