<template>
  <div class="post">
    <div>
      <div><strong>Id: </strong>{{ post.id }}</div>
      <div><strong>Название: </strong>{{ post.title }}</div>
      <div><strong>Описание: </strong>{{ post.body }}</div>
    </div>
    <div class="post__btns">
      <my-button-vue
        @click="$router.push(`/posts/${post.id}`)"
      >
        Открыть
      </my-button-vue>
      <my-button-vue
        @click="$emit('remove', post)"
      >
        Удалить
      </my-button-vue>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.post {
  padding: 15px;
  margin-top: 15px;
  border: 2px solid teal;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.post__btns {
  display: flex;
}
</style>
