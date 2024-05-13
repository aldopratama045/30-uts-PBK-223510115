<template>
  <main class="app">
    <header class="header">
      <nav class="menu">
        <ul>
          <li @click="activeMenu = 'post'"><a href="#">Post</a></li>
          <li @click="activeMenu = 'todos'"><a href="#">Todos</a></li>
        </ul>
      </nav>
    </header>

    <section v-if="activeMenu === 'todos'" class="todos-section">
      <!-- Bagian untuk fitur Todos -->
      <section class="greeting">
        <h2 class="title">Selamat Datang Di, <input class="text" type="text" placeholder="KEGIATANKU" v-model="name" /></h2>
      </section>

      <section class="create-todo">
        <h3>TULIS KEGIATANMU</h3>

        <form @submit.prevent="addTodo">
          <h4>Kegiatan apa yang anda buat?</h4>
          <input type="text" placeholder="Telusuri atau Ketik Kegiatanku" v-model="input_content" />

          <h4>Pilih Kegiatanmu Berada Saat ini</h4>

          <div class="options">
            <label>
              <input type="radio" name="category" value="business" v-model="input_category" />
              <span class="bubble business"></span>
              <div>INDOOR</div>
            </label>

            <label>
              <input type="radio" name="category" value="personal" v-model="input_category" />
              <span class="bubble personal"></span>
              <div>OUTDOOR</div>
            </label>
          </div>

          <input type="submit" value="Add Kegiatanku" />
        </form>
      </section>

      <section class="todo-list">
        <h3>KEGIATANKU</h3>
        <div class="list">
          <div v-for="todo in todos_asc" :key="todo.createdAt" :class="`todo-item ${todo.done && 'done'}`">
            <label>
              <input type="checkbox" v-model="todo.done" />
              <span :class="`bubble ${todo.category}`"></span>
            </label>

            <div class="todo-content">
              <input type="text" v-model="todo.content" />
            </div>

            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">HAPUS</button>
            </div>
          </div>
        </div>
      </section>
    </section>

    <section v-else class="post-section">
      <!-- Bagian untuk fitur Postingan -->
      <h2>Postingan Pengguna</h2>
      <div class="select-user">
        <label>Pilih Pengguna:</label>
        <select v-model="selectedUser" @change="fetchPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div v-if="posts.length > 0" class="user-posts">
        <h3>Postingan Pengguna: {{ selectedUserName }}</h3>
        <ul>
          <li v-for="post in posts" :key="post.id">
            <h4>{{ post.title }}</h4>
            <p>{{ post.body }}</p>
          </li>
        </ul>
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)
const activeMenu = ref('todos') // Menetapkan menu awal yang aktif

const todos_asc = computed(() => todos.value.sort((a, b) => b.createdAt - a.createdAt))

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })

  input_content.value = ''
  input_category.value = null
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

const selectedUser = ref(null)
const users = ref([])
const posts = ref([])
const selectedUserName = ref('')

const fetchUsers = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    const data = await response.json()
    users.value = data
  } catch (error) {
    console.error('Error fetching users:', error)
  }
}

const fetchPosts = async () => {
  if (!selectedUser.value) return
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`)
    const data = await response.json()
    posts.value = data
    selectedUserName.value = users.value.find(user => user.id === selectedUser.value)?.name || ''
  } catch (error) {
    console.error('Error fetching posts:', error)
  }
}

onMounted(fetchUsers)
</script>

<style scoped>

/* Font */
body {
  font-family: 'Roboto', sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f9f9f9;
}

/* Header */
.header {
  background-color: #4CAF50;
  color: white;
  padding: 20px 0;
}

.header h1 {
  margin: 0;
  text-align: center;
}

/* Menu */
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

.menu ul li a:hover {
  background-color: #388E3C;
}

.menu ul li.active a {
  background-color: #388E3C;
}

/* Sections */
.section {
  background-color: white;
  margin-top: 20px;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
}

.section h2 {
  margin-top: 0;
}

/* Form */
.form-container {
  margin-bottom: 20px;
}

.form-container input[type="text"],
.form-container select {
  width: calc(100% - 20px);
  padding: 10px;
  margin-top: 5px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

.form-container input[type="submit"] {
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 3px;
  padding: 10px 15px;
  cursor: pointer;
}

.form-container input[type="submit"]:hover {
  background-color: #388E3C;
}

/* Todo List */
.todo-item {
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 3px;
  background-color: #fafafa;
}

.todo-item.done {
  text-decoration: line-through;
  color: #888;
  background-color: #f0f0f0;
}

/* Bubble */
.bubble {
  display: inline-block;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  margin-right: 5px;
}

.bubble.business {
  background-color: #F44336;
}

.bubble.personal {
  background-color: #2196F3;
}

/* User Posts */
.user-posts {
  margin-top: 20px;
}

.user-posts h3 {
  margin-bottom: 10px;
}

.user-posts ul {
  list-style-type: none;
  padding: 0;
}

.user-posts li {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  margin-bottom: 10px;
  background-color: white;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
}

.user-posts li h4 {
  margin-top: 0;
}

.user-posts li p {
  margin-bottom: 0;
}
</style>
