<template>
  <div v-if="posts.length > 0">
    <h3>Список постов</h3>
    <transition-group name="posts-list">
      <post-item-vue
      v-for="post in posts"
      :key="post.id"
      :post="post"
      @remove="$emit('remove', post)"
    ></post-item-vue>
    </transition-group>
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

<style scoped>
.posts-list-item {
  display: inline-block;
  margin-right: 10px;
}
.posts-list-enter-active,
.posts-list-leave-active {
  transition: all 0.6s ease;
}
.posts-list-enter-from,
.posts-list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}
.posts-list-move {
  transition: transform 0.6s ease;
}
</style>
