<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input-vue
      placeholder="Найти по названию..."
      v-model="searchQuery"
      v-focus
    />
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
      :posts="sortedAndSearchedPosts"
      @remove="removePost"
      v-if="!isPostLoading"
    />
    <div v-else>Идет загрузка...</div>
    <div v-intersection="loadMorePosts" class="observer"></div>

    <!-- Реализация пагинации -->
    <!-- <div class="page__wrapper">
      <div
        class="page"
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        :class="{
          'current-page': pageNumber === page
        }"
        @click="changePage(pageNumber)"
      >{{ pageNumber }}</div>
    </div> -->
  </div>
</template>

<script>
import axios from "axios";
import PostFormVue from "../components/PostForm.vue";
import PostListVue from "../components/PostList.vue";

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
      searchQuery: "",
      selectedSort: "",
      page: 1,
      limit: 10,
      totalPages: 0,
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
          "https://jsonplaceholder.typicode.com/posts", {
            params: {
              _page: this.page,
              _limit: this.limit
            }
          }
        );
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
        this.posts = response.data;
      } catch (error) {
        console.error(error);
      } finally {
        this.isPostLoading = false;
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1;
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts", {
            params: {
              _page: this.page,
              _limit: this.limit
            }
          }
        );
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
        this.posts = [...this.posts, ...response.data];
      } catch (error) {
        console.error(error);
      }
    },
    //Реализация пагинации
    /*changePage(pageNumber) {
      this.page = pageNumber;
    } */
  },

  watch: {
    //Реализация пагинации
    /*page() {
      this.fetchPosts();
    } */
  },

  mounted() {
    this.fetchPosts();
    //Реализация дозагрузки постов при прокрутке до низа страницы
    /* const options = {
      rootMargin: '0px',
      threshold: 1.0
    }
    const callback = (entries, observer) => {
      if (entries[0].isIntersecting && this.page < this.totalPages) {
        this.loadMorePosts();
      }
    };
    const observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer); */
  },

  computed: {
    sortedPost() {
      return [...this.posts].sort((post1, post2) =>
        post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
      )
    },
    sortedAndSearchedPosts() {
      return this.sortedPost.filter((post) => post.title.toLowerCase().includes(this.searchQuery.toLocaleLowerCase()))
    }
  },
};
</script>

<style>
.page__wrapper {
  display: flex;
  justify-content: center;
  margin-top: 15px;
}
.page {
  border: 1px solid black;
  padding: 10px;
}
.current-page {
  border: 2px solid teal;
}
</style>
