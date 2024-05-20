<template>
  <main class="app">
    <header class="header">
      <nav class="menu">
        <ul>
          <li :class="{ active: activeMenu === 'post' }" @click="activeMenu = 'post'"><a href="#">Post</a></li>
          <li :class="{ active: activeMenu === 'todos' }" @click="activeMenu = 'todos'"><a href="#">Todos</a></li>
        </ul>
      </nav>
    </header>

    <Todos v-if="activeMenu === 'todos'">
      <template #link>
        <section class="link">
          <a href="https://example.com">Kunjungi situs Todos</a>
        </section>
      </template>
    </Todos>
    <Posts v-else title="User Posts" :initialUserId="1">
      <template #post="{ post }">
        <h4>{{ post.title }} (custom slot)</h4>
        <p>{{ post.body }}</p>
      </template>
      <template #link>
        <section class="link">
          <a href="https://example.com">Kunjungi situs Posts</a>
        </section>
      </template>
    </Posts>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import Todos from './components/Todos.vue'
import Posts from './components/Posts.vue'

const activeMenu = ref('todos') // Menetapkan menu awal yang aktif
</script>

<style scoped>
/* CSS code for App.vue */
body {
  font-family: 'Roboto', sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f9f9f9;
}

.header {
  background-color: #4CAF50;
  color: white;
  padding: 20px 0;
  text-align: center;
}

.menu ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  text-align: center;
}

.menu ul li {
  display: inline-block;
  margin-right: 20px;
}

.menu ul li a {
  color: white;
  text-decoration: none;
  padding: 10px 15px;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.menu ul li a:hover,
.menu ul li.active a {
  background-color: #388E3C;
}
</style>
