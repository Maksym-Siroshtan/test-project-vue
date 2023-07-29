<template>
  <div v-if="posts.length > 0">
    <h3>Список постов</h3>
    <post-item-vue
      v-for="post in posts"
      :key="post.id"
      :post="post"
      @remove="$emit('remove', post)"
    ></post-item-vue>
  </div>
  <h2 v-else style="color: red">Список постов пуст.</h2>
</template>

<script>
import PostItemVue from "./PostItem.vue";
export default {
  components: {
    PostItemVue,
  },
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped></style>
