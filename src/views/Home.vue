<template>
  <div class="home">
    <h1>New post</h1>
    Title:
    <input type="text" v-model="newPostTitle" />
    Body:
    <input type="text" v-model="newPostBody" />
    Image:
    <input type="text" v-model="newPostImage" />
    <button v-on:click="createPost()">Create</button>
    <h1>All posts</h1>
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>{{ post.title }}</h2>
      <div>
        <button v-on:click="showPost(post)">More info</button>
      </div>
    </div>
    <dialog id="post-details">
      <form method="dialog">
        <h1>Post info</h1>
        <p>
          Title:
          <input type="text" v-model="currentPost.title" />
        </p>
        <p>
          Body:
          <input type="text" v-model="currentPost.body" />
        </p>
        <p>
          Image:
          <input type="text" v-model="currentPost.image" />
        </p>
        <button v-if="currentPost.is_owner" v-on:click="updatePost(currentPost)">Update</button>
        <button v-if="currentPost.is_owner" v-on:click="destroyPost(currentPost)">Destroy</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      posts: [],
      currentPost: {},
      newPostTitle: "",
      newPostBody: "",
      newPostImage: "",
    };
  },
  created: function () {
    this.indexPosts();
  },
  methods: {
    indexPosts: function () {
      axios.get("http://localhost:3000/posts").then((response) => {
        this.posts = response.data;
        console.log("All posts", this.posts);
      });
    },
    createPost: function () {
      console.log("Create the post...");
      var params = {
        title: this.newPostTitle,
        body: this.newPostBody,
        image: this.newPostImage,
      };
      axios
        .post("http://localhost:3000/posts", params)
        .then((response) => {
          console.log(response.data);
          this.posts.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showPost: function (post) {
      console.log("hello", post);
      this.currentPost = post;
      document.querySelector("#post-details").showModal();
    },
    updatePost: function (post) {
      console.log("update post", post);
      var params = {
        title: post.title,
        body: post.body,
        image: post.image,
      };
      axios.patch("http://localhost:3000/posts/" + post.id, params).then((response) => {
        console.log("Update success", response.data);
      });
    },
    destroyPost: function (post) {
      console.log("destroy post", post);
      axios.delete("http://localhost:3000/posts/" + post.id).then((response) => {
        console.log("Destroy success", response.data);
        var index = this.posts.indexOf(post);
        this.posts.splice(index, 1);
      });
    },
  },
};
</script>
