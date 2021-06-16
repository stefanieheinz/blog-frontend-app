<template>
  <div>
    <h1>All Posts</h1>

    <div>
      Search by title:
      <input type="text" v-model="searchFilter" list="post-titles" />
      <datalist id="post-titles">
        <option v-for="post in posts" v-bind:key="post.id">{{ post.title }}</option>
      </datalist>
    </div>

    <div
      class="row"
      is="transition-group"
      appear
      enter-active-class="animated zoomInDown"
      leave-active-class="animated fadeOutLong"
    >
      <div class="col-sm-4" v-for="post in filterBy(posts, searchFilter, 'title')" v-bind:key="post.id">
        <h2>{{ post.title }}</h2>
        <a v-bind:href="`/posts/${post.id}`" class="btn btn-primary">More info</a>
      </div>
    </div>
  </div>
</template>

<style>
.fadeOutLong {
  opacity: 0;
  transition: opacity 2s ease;
}
</style>
<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      posts: [],
      searchFilter: "",
    };
  },
  created: function () {
    this.indexPosts();
  },
  methods: {
    indexPosts: function () {
      axios.get("/posts").then((response) => {
        console.log("index posts", response.data);
        this.posts = response.data;
      });
    },
  },
};
</script>
