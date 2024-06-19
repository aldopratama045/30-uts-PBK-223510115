<script setup>
import { ref, onMounted } from 'vue'

const selectedUser = ref(null)
const users = ref([])
const posts = ref([])
const selectedUserName = ref('')

const fetchUsers = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    users.value = await response.json()
  } catch (error) {
    console.error('Error fetching users:', error)
  }
}

const fetchPosts = async () => {
  if (!selectedUser.value) return
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`)
    posts.value = await response.json()
    selectedUserName.value = users.value.find(user => user.id === selectedUser.value)?.name || ''
  } catch (error) {
    console.error('Error fetching posts:', error)
  }
}

onMounted(fetchUsers)
</script>

<template>
  <section class="post-section">
    <h2>Postingan Pengguna</h2>
    <div class="select-user">
      <label>Pilih Pengguna:</label>
      <select v-model="selectedUser" @change="fetchPosts" class="select-box">
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
    </div>
    <div v-if="posts.length > 0" class="user-posts">
      <h3>Postingan Pengguna: {{ selectedUserName }}</h3>
      <table class="post-table">
        <thead>
          <tr>
            <th>Judul</th>
            <th>Isi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="post in posts" :key="post.id">
            <td>{{ post.title }}</td>
            <td>{{ post.body }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<style scoped>
body {
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-image: url('https://images.unsplash.com/photo-1517816986127-4d96c9a8d3b5?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80');
  background-size: cover;
  background-position: center;
  font-family: 'Arial', sans-serif;
}

.post-section {
  background-color: rgba(255, 255, 255, 0.8);
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  max-width: 800px;
  width: 100%;
}

.select-user {
  margin-bottom: 20px;
}

.select-box {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f8f8f8;
  transition: border-color 0.3s, background-color 0.3s;
}

.select-box:focus {
  outline: none;
  border-color: #007bff;
  background-color: white;
}

.user-posts {
  margin-top: 20px;
}

.post-table {
  width: 100%;
  border-collapse: collapse;
}

.post-table th,
.post-table td {
  padding: 15px;
  border: 1px solid #ddd;
  text-align: left;
}

.post-table th {
  background-color: #f4f4f4;
  font-weight: bold;
}

.post-table tr:nth-child(even) {
  background-color: #f9f9f9;
}

h2, h3, label {
  color: #333;
  margin-bottom: 10px;
}
</style>
