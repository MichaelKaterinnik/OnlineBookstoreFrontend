<template>
  <div class="books-by-author">
    <div class="search-results-header">
      <h2>Книги автора {{ authorName }}</h2>
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
    <div class="books-list">
      <ul>
        <li v-for="book in books" :key="book.id">
          <router-link :to="'/books/get/' + book.id">
            <div class="book-item">
              <div class="book-cover">
                <img :src="book.imageUrl" :alt="book.name">
              </div>
              <div class="book-details">
                <h3>{{ book.name }}</h3>
                <p>{{ book.authorName }}</p>
              </div>
            </div>
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'BooksByAuthor',
  data() {
    return {
      author: '',
      books: [],
      authenticated: false,
      bookSearchQuery: ''
    };
  },
  created() {
    this.author = this.$route.params.author;
    this.authenticated = this.$store.getters.isAuthenticated;
    this.searchBooks();
  },
  methods: {
    searchBooks() {
      axios.get(`/books/books_by_author?author=${this.author}`)
          .then(response => {
            this.books = response.data;
          })
          .catch(error => {
            console.log(error);
          });
    },
    searchBooksByTitle() {
      this.$router.push(`/books/search?query=${this.bookSearchQuery}`);
    }
  }
};
</script>

<style>
.books-by-author {
  margin-top: 50px;
}

.books-by-author-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.books-by-author-header h2 {
  font-size: 24px;
  font-weight: 700;
}

.books-by-author-header .search-books {
  display: flex;
  align-items: center;
}

.books-by-author-header .search-books input[type="text"] {
  padding: 5px;
  margin-right: 10px;
  width: 300px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

.books-by-author-header .search-btn {
  padding: 5px 10px;
  background-color: #337ab7;
  border: none;
  border-radius: 3px;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}

.books-by-author-header .search-btn:hover {
  background-color: #286090;
}

.book-list {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 30px;
}

.book-list li {
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid #ccc;
  border-radius: 3px;
  padding: 10px;
  background-color: #f5f5f5;
  height: 350px;
}

.book-list li:hover {
  background-color: #eee;
}

.book-list li a {
  text-decoration: none;
  color: #337ab7;
  font-size: 18px;
  margin-top: 10px;
}

.book-list li img {
  max-width: 150px;
  max-height: 200px;
  margin-bottom: 10px;
}

.book-list li h3 {
  font-size: 16px;
  font-weight: 700;
  margin-top: 10px;
  margin-bottom: 5px;
}

.book-list li h4 {
  font-size: 16px;
  font-weight: 400;
  margin-top: 0;
  margin-bottom: 5px;
}

.book-list li p {
  font-size: 14px;
  line-height: 1.4;
  text-align: center;
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
