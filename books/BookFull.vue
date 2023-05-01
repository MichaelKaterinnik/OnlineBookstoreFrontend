<template>
  <div class="book">
    <div class="search-bar">
      <input type="text" v-model="searchQuery" placeholder="Пошук за ім'ям автора">
      <button class="search-btn" @click="searchAuthors">Пошук авторів</button>
    </div>
    <div class="search-bar">
      <input type="text" v-model="bookSearchQuery" placeholder="Пошук за назвою книги">
      <button class="search-btn" @click="searchBooks">Пошук книг</button>
    </div>
    <div class="book-header">
      <h2>{{ book.title }}</h2>
      <p>{{ book.author.name }}</p>
    </div>
    <div class="book-body">
      <div class="book-image">
        <img :src="book.coverImage" :alt="book.title">
      </div>
      <div class="book-details">
        <p>{{ book.description }}</p>
        <p>Рейтинг: {{ book.rating }}</p>
        <p>Ціна: {{ book.price }}</p>
        <p v-if="book.availability">Наявність: В наявності</p>
        <p v-else>Наявність: Немає в наявності</p>
        <button class="add-to-cart-btn" @click="addToCart">Додати до замовлення</button>
        <div class="book-actions" v-if="isAuthenticated">
          <button class="add-to-wishlist-btn">Додати до списку бажаних</button>
          <button class="add-review-btn">Залишити відгук</button>
        </div>
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
  name: 'Book',
  data() {
    return {
      book: {},
      isAuthenticated: false,
      cartItemCount: 0
    }
  },
  created() {
    const bookId = this.$route.params.id;
    axios.get(`/books/get/${bookId}`)
        .then(response => {
          this.book = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    const token = localStorage.getItem('token');
    if (token) {
      this.isAuthenticated = true;
    }
    // implement a method to get the cart item count
    this.cartItemCount = this.getCartItemCount();
  },
  methods: {
    addToCart() {
      // implement a method to add the book to the cart
      this.addToCart(this.book);
      this.cartItemCount++;
    },
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

<style>
.logo img {
  height: 40px;
  margin-right: 10px;
}
.auth-btns button {
  margin-left: 10px;
  background-color: #fff;
  border: 1px solid #333;
  padding: 5px 15px;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
}
.book-header {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: flex-start;
  margin-top: 2rem;
}
.book-image {
  width: 25rem;
  height: 35rem;
  background-size: cover;
  background-position: center;
  margin-right: 2rem;
}

/* Book info */
.book-details {
  flex-basis: calc(75% - 2rem);
}
.book-title {
  font-size: 3rem;
  margin-bottom: 1rem;
}
.book-author-name {
  font-size: 1.6rem;
  margin-bottom: 1rem;
}
.book-rating {
  font-size: 2rem;
  margin-bottom: 1rem;
}
.book-price {
  font-size: 2rem;
  margin-bottom: 1rem;
}
.book-quantity {
  font-size: 1.6rem;
  margin-bottom: 1rem;
}
.book-availability {
  font-size: 1.6rem;
  margin-bottom: 1rem;
}
/* Adding to basket, wishlist, review buttons */
.book-actions {
  margin-top: 2rem;
}
/* Auth + go to personal cabinet buttons */
.login-btn {
  margin-top: 2rem;
  display: flex;
  justify-content: flex-end;
}
/* Basket ref button */
.cart-btn {
  font-size: 1.6rem;
  margin-top: 2rem;
}
</style>