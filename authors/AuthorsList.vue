<template>
  <div class="author-search-results">
    <div class="search-results-header">
      <h2>Список авторів</h2>
      <div class="search-results-actions">
        <router-link :to="isAuthenticated ? '/cabinet' : '/login'">
          <button class="login-btn">{{ isAuthenticated ? 'Особистий кабінет' : 'Увійти' }}</button>
        </router-link>
        <router-link to="/user_order">Корзина покупок</router-link>
        <div class="search-books">
          <input type="text" v-model="bookSearchQuery" placeholder="Пошук книг за назвою">
          <button class="search-btn" @click="searchBooks">Пошук</button>
        </div>
      </div>
    </div>
    <div class="author-list">
      <ul>
        <li v-for="author in authors" :key="author.id">
          <router-link :to="'/books/books_by_author/' + author.id">{{ author.lastName }} {{ author.firstName }}</router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'AuthorList',
  data() {
    return {
      searchQuery: '',
      authors: [],
      authenticated: false,
      bookSearchQuery: ''
    };
  },
  created() {
    this.authenticated = this.$store.getters.isAuthenticated;
    this.getAuthors();
  },
  methods: {
    getAuthors() {
      axios.get('/authors/get_all')
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
};
</script>

<style>
.search-results {
  display: flex;
  flex-direction: column;
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

.search-btn {
  padding: 5px 10px;
  background-color: #337ab7;
  border: none;
  border-radius: 3px;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}

.search-btn:hover {
  background-color: #286090;
}

.author-list {
  width: 600px;
  margin: 0 auto;
}

.author-list ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.author-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 3px;
  margin-bottom: 10px;
  background-color: #f5f5f5;
}

.author-list li:hover {
  background-color: #eee;
}

.author-list li a {
  text-decoration: none;
  color: #337ab7;
  font-size: 18px;
}

.author-list li a:hover {
  text-decoration: underline;
}

.functional-buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.functional-buttons a,
.functional-buttons button {
  margin-right: 10px;
  padding: 5px 10px;
  background-color: #337ab7;
  border: none;
  border-radius: 3px;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
  text-decoration: none;
}

.functional-buttons a:hover,
.functional-buttons button:hover {
  background-color: #286090;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.pagination button {
  margin-right: 10px;
  padding: 5px 10px;
  background-color: #fff;
  border: 1px solid #337ab7;
  border-radius: 3px;
  color: #337ab7;
  font-size: 16px;
  cursor: pointer;
}

.pagination button.active {
  background-color: #337ab7;
  color: #fff;
}
</style>
