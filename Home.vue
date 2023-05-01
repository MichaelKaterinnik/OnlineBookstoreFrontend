<template>
  <div class="home">
    <div class="search-bar">
      <input type="text" v-model="searchQuery" placeholder="Пошук за ім'ям автора">
      <button class="search-btn" @click="searchAuthors">Пошук авторів</button>
    </div>
    <div class="search-bar">
      <input type="text" v-model="bookSearchQuery" placeholder="Пошук за назвою книги">
      <button class="search-btn" @click="searchBooks">Пошук книг</button>
    </div>
    <div class="book-list">
      <h2>Популярні книги</h2>
      <div class="book-item" v-for="book in books" :key="book.id">
        <router-link :to="'/books/get/' + book.id">
          <img :src="book.coverImage" :alt="book.title">
          <h3>{{ book.title }}</h3>
          <p>{{ book.author.name }}</p>
        </router-link>
      </div>
    </div>
    <div class="categories">
      <h2>Категорії книг</h2>
      <ul>
        <li v-for="category in categories" :key="category">
          <router-link :to="'/books/collection_books/' + { name: 'Category', params: { name: category } }">{{ category }}</router-link>
        </li>
      </ul>
    </div>
    <div class="book-footer">
      <router-link class="back-btn" to="/">На головну</router-link>
      <router-link class="cart-btn" to="/user_order">Кошик ({{ cartItemCount }})</router-link>
      <router-link :to="isAuthenticated ? '/cabinet' : '/login'">
        <button class="login-btn">{{ isAuthenticated ? 'Особистий кабінет' : 'Увійти' }}</button>
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      books: [],
      categories: []
    }
  },
  created() {
        axios.get('/').then(response => {
          this.books = response.data.books;
          this.categories = response.data.categories;
        });
  },
  methods: {
    searchAuthors() {
      axios.get(`/authors/get/${this.searchQuery}`)
          .then(response => {
            this.authors = response.data;
          })
          .catch(error => {
            console.log(error);
          });
    },
    searchBooks() {
      this.$router.push(`/books/books_search${this.bookSearchQuery}`);
    }
  }
}
</script>

<style scoped>
.home {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  padding: 20px;
}
.search-bar {
  width: 100%;
  margin-bottom: 20px;
}
.book-list {
  width: 70%;
}
.book-item {
  margin-bottom: 20px;
}
.book-item img {
  width: 100%;
  height: auto;
  margin-bottom: 10px;
}
.categories {
  width: 30%;
}
.categories ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.categories li {
  margin-bottom: 10px;
}
.login-btn {
  background-color: #2196F3;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
}
</style>