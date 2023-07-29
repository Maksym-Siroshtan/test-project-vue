<template>
  <div class="app">
    <h1>Страница с постами</h1>

    <div class="app__btns">
      <my-button-vue @click="showDialog">
        Создать пост
      </my-button-vue>

      <my-select-vue
        v-model="selectedSort"
        :options="sortOptions"
      />
    </div>

    <my-dialog-vue v-model:show="dialogVisible">
      <post-form-vue @create="createPost" />
    </my-dialog-vue>

    <post-list-vue
      :posts="posts"
      @remove="removePost"
      v-if="!isPostLoading"
    />
    <div v-else>Идет загрузка...</div>
  </div>
</template>

<script>
import axios from "axios";
import PostFormVue from "./components/PostForm.vue";
import PostListVue from "./components/PostList.vue";

export default {
  components: {
    PostFormVue,
    PostListVue,
  },

  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: "",
      sortOptions: [
        { value: "title", name: "По названию" },
        { value: "body", name: "По содержимому" },
      ],
    };
  },

methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true;
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts?_limit=10"
        );
        this.posts = response.data;
      } catch (error) {
        console.error(error);
      } finally {
        this.isPostLoading = false;
      }
    },

  },

  mounted() {
    this.fetchPosts();
  },

  watch: {
    selectedSort(newValue) {
      console.log(newValue);
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.app {
  padding: 20px;
}
.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
</style>
