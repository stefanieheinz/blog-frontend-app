<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <a class="navbar-brand" href="#">Bloggr</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <router-link class="nav-link" to="/">Home</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/posts">All Posts</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/posts/new">New Post</router-link>
          </li>
          <li class="nav-item" v-if="!isLoggedIn()">
            <router-link class="nav-link" to="/signup">Signup</router-link>
          </li>
          <li class="nav-item" v-if="!isLoggedIn()">
            <router-link class="nav-link" to="/login">Login</router-link>
          </li>
          <li class="nav-item" v-if="isLoggedIn()">
            <router-link class="nav-link" to="/logout">Logout</router-link>
          </li>
        </ul>
        <form class="form-inline my-2 my-lg-0">
          <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search" />
          <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
        </form>
      </div>
    </nav>
    <div class="container">
      <div v-if="flashMessage" v-on:click="flashMessage = null" class="alert alert-success" role="alert">
        {{ flashMessage }}
      </div>
      <router-view />
    </div>
  </div>
</template>
<style>
body {
  background-image: url("./assets/notebook.png");
}
#app {
  font-family: Futura, Trebuchet MS, Arial, sans-serif;
}
.navbar-brand {
  font-family: "Monoton", cursive;
  font-size: 6em;
}
</style>

<script>
export default {
  data: function () {
    return {
      flashMessage: null,
    };
  },
  methods: {
    isLoggedIn: function () {
      return localStorage.getItem("jwt");
    },
  },
};
</script>
