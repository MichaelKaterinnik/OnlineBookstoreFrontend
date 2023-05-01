<template>
  <div class="book-search">
    <div class="search-bar">
      <input type="text" v-model="searchQuery" placeholder="Пошук за назвою книги">
      <button class="search-btn" @click="searchBooks">Пошук</button>
      <input type="text" v-model="authorSearchQuery" placeholder="Пошук за ім'ям автора">
      <button class="search-btn" @click="searchAuthors">Пошук</button>
    </div>
    <div class="book-list">
      <div class="book-card" v-for="book in books" :key="book.id">
        <router-link :to="'/books/get/' + book.id">
          <div class="book-image">
            <img :src="book.coverImage" :alt="book.title">
          </div>
          <div class="book-info">
            <h3>{{ book.title }}</h3>
            <p>{{ book.author.name }}</p>
          </div>
        </router-link>
      </div>
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
import axios from 'axios';

export default {
  name: 'BookSearch',
  data() {
    return {
      searchQuery: '',
      authorSearchQuery: '',
      books: [],
      isAuthenticated: false,
      cartItemCount: 0
    };
  },
  created() {
    this.searchQuery = this.$route.params.query;
    this.authenticated = this.$store.getters.isAuthenticated;
    this.searchBooks();
    // implement a method to get the cart item count
    this.cartItemCount = this.getCartItemCount();
  },
  methods: {
    searchBooks() {
      axios.get(`/books/books_search${this.searchQuery}`)
          .then(response => {
            this.books = response.data;
          })
          .catch(error => {
            console.log(error);
          });
    },
    searchAuthors() {
      axios.get(`/authors/get/${this.authorSearchQuery}`)
          .then(response => {
            this.authors = response.data;
          })
          .catch(error => {
            console.log(error);
          });
    }
  }
};
</script>

<style>
.book-search {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 50px;
}

.search-bar {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.search-bar input[type="text"] {
  padding: 5px;
  margin-right: 10px;
  width: 300px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

.search-btn,
.show-all-btn {
  padding: 5px 10px;
  background-color: #337ab7;
  border: none;
  border-radius: 3px;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}

.search-btn:hover,
.show-all-btn:hover {
  background-color: #286090;
}

.book-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 50px;
}

.book-card {
  width: 300px;
  margin: 20px;
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 3px;
  overflow: hidden;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.3);
}

.book-card:hover {
  box-shadow: 0 4px 4px rgba(0, 0, 0, 0.3);
}

.book-image {
  height: 200px;
  background-size: cover;
  background-position: center;
}

.book-info {
  padding: 10px;
}
.book-title {
  font-size: 18px;
  margin-bottom: 5px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.book-author {
  font-size: 14px;
  color: #999;
  margin-bottom: 5px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.book-actions {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}
.add-to-cart-btn {
  padding: 5px 10px;
  background-color: #337ab7;
  border: none;
  border-radius: 3px;
  color: #fff;
  font-size: 14px;
  cursor: pointer;
}
.add-to-cart-btn:hover {
  background-color: #286090;
}
.view-details-btn {
  padding: 5px 10px;
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 3px;
  color: #333;
  font-size: 14px;
  cursor: pointer;
}
.view-details-btn:hover {
  background-color: #e6e6e6;
}
</style>
