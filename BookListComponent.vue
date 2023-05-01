<template>
  <div>
    <h2>Список книг</h2>
    <ul>
      <li v-for="book in books" :key="book.id">
        <router-link :to="{ name: 'book', params: { id: book.id } }">
          <h3>{{ book.title }}</h3>
          <p>{{ book.author }}</p>
          <img :src="book.coverImage" :alt="book.title">
        </router-link>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'BooksListComponent',
  props: {
    books: Array
  }
}
</script>
<style scoped>
h2 {
  font-size: 24px;
  font-weight: bold;
}
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
li {
  margin-bottom: 20px;
}
h3 {
  font-size: 20px;
  font-weight: bold;
  margin: 0;
}
p {
  font-size: 16px;
  color: gray;
  margin: 5px 0;
}
img {
  max-width: 200px;
  height: auto;
}
</style>