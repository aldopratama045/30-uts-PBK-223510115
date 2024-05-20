<template>
    <section class="todos-section">
      <slot name="greeting">
        <section class="greeting">
          <h2 class="title">
            Selamat Datang Di, <input class="text" type="text" placeholder="KEGIATANKU" v-model="name" />
          </h2>
        </section>
      </slot>
      <section class="create-todo">
        <h3>TULIS KEGIATANMU</h3>
        <form @submit.prevent="addTodo">
          <h4>Kegiatan apa yang anda buat?</h4>
          <input type="text" placeholder="Telusuri atau Ketik Kegiatanku" v-model="inputContent" />
          <h4>Pilih Kegiatanmu Berada Saat ini</h4>
          <div class="options">
            <label>
              <input type="radio" name="category" value="business" v-model="inputCategory" />
              <span class="bubble business"></span>
              <div>INDOOR</div>
            </label>
            <label>
              <input type="radio" name="category" value="personal" v-model="inputCategory" />
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
          <div v-for="todo in todosAsc" :key="todo.createdAt" :class="['todo-item', { done: todo.done }]">
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
      <slot name="link">
        <section class="link">
          <a href="https://example.com">Kunjungi situs Todos</a>
        </section>
      </slot>
    </section>
  </template>
  
  <script setup>
  import { ref, onMounted, computed, watch } from 'vue'
  
  const name = ref('')
  const todos = ref([])
  const inputContent = ref('')
  const inputCategory = ref(null)
  
  const todosAsc = computed(() => todos.value.sort((a, b) => b.createdAt - a.createdAt))
  
  const addTodo = () => {
    if (inputContent.value.trim() === '' || inputCategory.value === null) {
      return
    }
  
    todos.value.push({
      content: inputContent.value,
      category: inputCategory.value,
      done: false,
      createdAt: new Date().getTime()
    })
  
    inputContent.value = ''
    inputCategory.value = null
  }
  
  const removeTodo = todo => {
    todos.value = todos.value.filter(t => t !== todo)
  }
  
  watch(todos, newVal => {
    localStorage.setItem('todos', JSON.stringify(newVal))
  }, { deep: true })
  
  watch(name, newVal => {
    localStorage.setItem('name', newVal)
  })
  
  onMounted(() => {
    name.value = localStorage.getItem('name') || ''
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })
  </script>
  
  <style scoped>
  /* CSS code for Todos.vue */
  .todos-section .greeting {
    margin-bottom: 20px;
  }
  
  .todos-section input[type="text"] {
    width: 100%;
    padding: 10px;
    margin: 5px 0 10px 0;
    border: 1px solid #ccc;
    border-radius: 3px;
  }
  
  .create-todo {
    margin-bottom: 20px;
  }
  
  .options {
    margin-bottom: 20px;
  }
  
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
  
  .todo-list .todo-item {
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
  
  .todo-list .actions {
    margin-left: auto;
  }
  
  .todo-list .actions .delete {
    background-color: #F44336;
    color: white;
    border: none;
    border-radius: 3px;
    padding: 5px 10px;
    cursor: pointer;
  }
  
  .todo-list .actions .delete:hover {
    background-color: #D32F2F;
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
  