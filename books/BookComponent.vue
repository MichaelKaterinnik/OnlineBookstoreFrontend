<template>
  <div>
    <h2>{{ title }}</h2>
    <p>{{ description }}</p>
  </div>
</template>

<script>
export default {
  name: 'BookComponent',
  props: {
    title: String,
    description: String
  },
  data() {
    return {
      book: {}
    }
  },
  methods: {
    getBook() {
      // Отримати книгу з бекенду за допомогою API
    }
  }
}
</script>

<style>
h2 {
  font-size: 24px;
  font-weight: bold;
}
p {
  font-size: 16px;
  color: gray;
}
</style>
