<template>
  <div class="post">
    <h1>Post</h1>
    <div class="create">
      <label for="create-post" class="create-post">Say Something....</label>
      <input type="text" id="create-post" v-model="text" placeholder="Create a Post">
      <button v-on:click="createPost">Post!</button>
    </div>
    <!-- Create Post -->
    <hr>
    <p class="err" v-if="error">{{ error }}</p>
    <div class="post-container">
      <div class="post-new" v-for="(post, index) in posts" :key="post._id">
        {{ formatDate(post.createdAt) }}
        <p class="text">{{ post.text }}</p>
        <button v-on:click="deletePost(post._id)">Delete</button> <!-- Add delete button -->
      </div>
    </div>
  </div>
</template>

<script>
import PostService from '../PostService'

export default {
  data() {
    return {
      posts: [],
      error: '',
      text: '' // Update variable name to 'text'
    }
  },
  async created() {
    try {
      this.posts = await PostService.getPosts();
    } catch (err) {
      this.error = err.message;
    }
  },
  methods: {
    async createPost() {
      await PostService.insertPost(this.text);
      this.posts = await PostService.getPosts();
      this.text = ''; // Clear the input field after posting
    },
    async deletePost(id) { // Pass the post ID to the deletePost method
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      const formattedDate = `${date.getDate()}/${date.getMonth() + 1}/${date.getFullYear()}`;
      return formattedDate;
    }
  }
}
</script>

<style scoped>
.post h1 {
  color: black;
  align-content: center;
}
.post-new {
  width: 50%;
  background-color: aquamarine;
  height: 20vh;
  align-items: center;
}
</style>
