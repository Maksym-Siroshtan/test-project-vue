<template>
  <div>
    <h1 class="about__title"><strong>В приложении разобрали несколько возможностей Vue.js для получения данных с <a href="https://jsonplaceholder.typicode.com/">JSONPlaceholder</a></strong></h1>
    <ol class="about__list">
      <li>
        <p><strong>Посты: </strong>получение постов с помощью Options Api</p>
      </li>
      <li>
        <p><strong>Store: </strong>получение постов с помощью хранилища данных Vuex</p>
      </li>
      <li>
        <p><strong>Composition Api: </strong>получение постов с помощью Composition Api</p>
      </li>
    </ol>
  </div>
</template>

<script>
  export default {

  }
</script>

<style scoped>
.about__title {
  margin-bottom: 20px;
}
.about__list {
  margin-left: 20px;
  font-size: 18px;
}
</style>