<template>
  <div class="">
    <div v-if="loading" class="mx-auto text-lg text-center items-center">
      Loading...
    </div>
    <div
      v-if="error"
      class="mx-auto text-lg items-center bg-red-500 p-6 text-white"
    >
      Sorry there was an error
    </div>
    <div v-for="post in posts" :key="post.id" class="mb-3 pb-3 border-b">
      <h3 class="text-lg font-semibold">
        <a
          v-if="post.url"
          :href="post.url"
          target="_blank"
          noreferrer=""
          nofollow=""
        >
          {{ post.title }}
        </a>
        <router-link
          v-else
          :to="{ name: 'PostDetail', params: { id: post.id } }"
          >{{ post.title }}
        </router-link>
      </h3>
      <div class="text-sm flex items-center">
        <span class="mr-3">
          {{ post.author }}
        </span>
        <router-link
          class="text-blue-800 hover:text-blue-600"
          v-if="post.url"
          :to="{ name: 'PostDetail', params: { id: post.id } }"
          >Link
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      loading: false,
      error: false,
      posts: []
    };
  },
  async created() {
    this.loading = true;
    try {
      const response = await axios.get("/api/posts/");
      this.posts = response.data;
    } catch (e) {
      this.error = true;
    } finally {
      this.loading = false;
    }
  }
};
</script>
