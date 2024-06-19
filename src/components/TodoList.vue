<template>
  <div class="body">
    <section class="create-todo">
      <h3>KETIK KEGIATAN MU HARI INI!</h3>
      <form @submit.prevent="addTodo">
        <h4>MASUKAN KEGIATAN</h4>
        <input type="text" placeholder="Masukkan kegiatan" v-model="inputContent" />
        <h4>Pilih kategori:</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" value="iNDOOR" v-model="inputCategory" />
            <span class="bubble home"></span>
            <div>INDOOR</div>
          </label>
          <label>
            <input type="radio" name="category" value="OUTDOOR" v-model="inputCategory" />
            <span class="bubble work"></span>
            <div>OUTDOOR</div>
          </label>
        </div>
        <button type="submit">Submit</button>
      </form>
      <div class="todo-list">
        <table>
          <thead>
            <tr>
              <th>Kegiatan</th>
              <th>Kategori</th>
              <th>Tombol</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="todo in todosAsc" :key="todo.createdAt" :class="`todo-item ${todo.done ? 'done' : ''}`">
              <td>{{ todo.content }}</td>
              <td>{{ todo.category }}</td>
              <td>
                <button class="delete" @click="removeTodo(todo)">
                  <i class="fa fa-trash"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const todos = ref([])
const inputContent = ref('')
const inputCategory = ref(null)

const todosAsc = computed(() => todos.value.slice().sort((a, b) => b.createdAt - a.createdAt))

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

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css');

.create-todo h3 {
  font-size: 30px;
  font-weight: bold;
  font-family: 'Roboto', sans-serif;
  color: #333;
}

.create-todo, .todo-list {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
  padding: 20px;
  font-family: 'Roboto', sans-serif;
  background-color: #f0f4f8;
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.create-todo form {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #ffffff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  transition: transform 0.3s;
}

.create-todo form:hover {
  transform: scale(1.05);
}

.create-todo .options {
  display: flex;
  justify-content: center;
  margin: 10px 0;
}

.create-todo .options label {
  margin: 0 10px;
  display: flex;
  align-items: center;
  cursor: pointer;
}

.create-todo .options input[type="radio"] {
  display: none;
}

.create-todo .options .bubble {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  margin-right: 10px;
  border: 2px solid transparent;
  transition: all 0.3s;
}

.create-todo .options input[type="radio"]:checked + .bubble {
  border-color: #007bff;
}

.create-todo .options .home { background: #ff6f61; }
.create-todo .options .work { background: #6fa3ef; }

button[type="submit"] {
  padding: 10px 20px;
  border: none;
  background: #007bff;
  color: white;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.3s, transform 0.3s;
  font-family: 'Roboto', sans-serif;
}

button[type="submit"]:hover {
  background: #0056b3;
  transform: translateY(-2px);
}

.todo-list table {
  width: 100%;
  border-collapse: collapse;
  font-family: 'Roboto', sans-serif;
  background-color: #fff;
}

.todo-list th, .todo-list td {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  color: #333;
}

.todo-list th {
  background-color: #f2f2f2;
}

.todo-list tr:hover {
  background-color: #f5f5f5;
  transition: background 0.3s;
}

.todo-list .todo-item.done {
  background: #e0ffe0;
}

.todo-list .todo-item .delete {
  background: transparent;
  border: none;
  color: #dc3545;
  cursor: pointer;
  transition: color 0.3s;
  font-size: 1.2em;
}

.todo-list .todo-item .delete:hover {
  color: #c82333;
}

.body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
  background-size: cover;
  background-position: center;
}
</style>
