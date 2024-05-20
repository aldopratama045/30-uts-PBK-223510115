<template>
    <section class="post-section">
      <h2>{{ title }}</h2>
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
            <slot name="post" :post="post">
              <h4>{{ post.title }}</h4>
              <p>{{ post.body }}</p>
            </slot>
          </li>
        </ul>
      </div>
      <slot name="link">
        <section class="link">
          <a href="https://example.com">Kunjungi situs Posts</a>
        </section>
      </slot>
    </section>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { defineProps, defineEmits } from 'vue'
  
  const props = defineProps({
    title: {
      type: String,
      default: 'Posts'
    },
    initialUserId: {
      type: Number,
      default: 1
    }
  })
  
  const selectedUser = ref(props.initialUserId)
  const users = ref([])
  const posts = ref([])
  const selectedUserName = ref('')
  
  const fetchUsers = async () => {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    users.value = await response.json()
  }
  
  const fetchPosts = async () => {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`)
    posts.value = await response.json()
    const user = users.value.find(user => user.id === selectedUser.value)
    selectedUserName.value = user ? user.name : ''
  }
  
  onMounted(async () => {
    await fetchUsers()
    await fetchPosts()
  })
  </script>
  
  <style scoped>
  /* CSS code for Posts.vue */
  .post-section {
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .select-user {
    margin-bottom: 20px;
  }
  
  .user-posts ul {
    list-style-type: none;
    padding: 0;
  }
  
  .user-posts li {
    margin-bottom: 15px;
  }
  
  .link {
    margin-top: 20px;
    text-align: center;
  }
  
  .link a {
    color: #2196F3;
    text-decoration: none;
  }
  
  .link a:hover {
    text-decoration: underline;
  }
  </style>
  