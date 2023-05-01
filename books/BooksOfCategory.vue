<template>
  <div class="category">
    <div class="search-bar">
      <input type="text" v-model="searchQuery" placeholder="Пошук за ім'ям автора">
      <button class="search-btn" @click="searchAuthors">Пошук авторів</button>
    </div>
    <div class="search-bar">
      <input type="text" v-model="bookSearchQuery" placeholder="Пошук за назвою книги">
      <button class="search-btn" @click="searchBooks">Пошук книг</button>
    </div>
    <h2>{{ categoryName }}</h2>
    <div class="book-list">
      <div class="book-item" v-for="book in books" :key="book.id">
        <router-link :to="{ name: 'Book', params: { id: book.id } }">
          <img :src="book.coverImage" :alt="book.title">
          <h3>{{ book.title }}</h3>
          <p>{{ book.author.name }}</p>
        </router-link>
      </div>
    </div>
    <div class="footer">
      <router-link class="back-btn" to="/">На головну</router-link>
      <router-link class="cart-btn" to="/user_order">Кошик ({{ cartItemCount }})</router-link>
      <router-link :to="isAuthenticated ? '/cabinet' : '/login'">
        <button class="login-btn">{{ isAuthenticated ? 'Особистий кабінет' : 'Увійти' }}</button>
      </router-link>
    </div>  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Category',
  data() {
    return {
      categoryName: '',
      books: []
    }
  },
  created() {
    const category = this.$route.params.name;
    axios.get(`/books/collection_books?collectionName=${category}`)
        .then(response => {
          this.categoryName = category;
          this.books = response.data;
        })
        .catch(error => {
          console.log(error);
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
.search-bar {
  width: 100%;
  margin-bottom: 20px;
}
.category {
  padding: 20px;
}
.book-list {
  margin-top: 20px;
}
.book-item {
  margin-bottom: 20px;
}
.book-item img {
  width: 100%;
  height: auto;
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
