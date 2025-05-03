<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="addTodo" class="form">
      <input v-model="newTodo" placeholder="Masukkan kegiatan..." />
      <button type="submit">Tambah</button>
    </form>

    <div class="dropdown-filter">
      <select id="filter-select" v-model="filter">
        <option value="all">Semua Kegiatan</option>
        <option value="pending">Belum Selesai</option>
        <option value="completed">Sudah Selesai</option>
      </select>
    </div>

    <ul>
      <li
        v-for="(todo, index) in filteredTodos"
        :key="'todo-' + index"
        :class="{ done: todo.completed }"
      >
        <input type="checkbox" v-model="todo.completed" />
        <span>{{ todo.text }}</span>
        <button class="delete" @click="removeTodo(index)">Batal</button>
      </li>
    </ul>

    <footer class="footer">
      &copy; {{ new Date().getFullYear() }} Sahira Ayiska
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const filter = ref('all')

const todos = ref([
  { text: 'belajar matematika', completed: false },
  { text: 'berbelanja', completed: false },
  { text: 'melukis', completed: true }
])

const addTodo = () => {
  if (newTodo.value.trim() === '') return
  todos.value.push({ text: newTodo.value.trim(), completed: false })
  newTodo.value = ''
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  if (filter.value === 'pending') {
    return todos.value.filter(todo => !todo.completed)
  } else if (filter.value === 'completed') {
    return todos.value.filter(todo => todo.completed)
  } else {
    return todos.value
  }
})
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 40px auto;
  background: linear-gradient(135deg, #ffeef8, #e0e5ff);
  border-radius: 20px;
  padding: 32px;
  box-shadow: 0 12px 28px rgba(0, 0, 0, 0.1);
  font-family: sans-serif;
}

h1 {
  text-align: center;
  color: #6a4c92;
  font-size: 2.5rem;
  font-weight: 600;
  margin-bottom: 20px;
}

.form {
  display: flex;
  justify-content: space-between;
  gap: 14px;
  margin-bottom: 20px;
}

input[type="text"] {
  padding: 14px;
  font-size: 16px;
  flex: 1;
  border-radius: 12px;
  border: 1px solid #e1b8e1;
  background-color: #fff;
  transition: 0.3s ease;
}

input[type="text"]:focus {
  border-color: #f3a6cb;
  outline: none;
  box-shadow: 0 0 8px rgba(243, 166, 203, 0.4);
}

button {
  background-color: #f3a6cb;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 12px;
  cursor: pointer;
  font-weight: 600;
  transition: 0.3s;
}

button:hover {
  background-color: #f1778a;
}

.dropdown-filter {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

select {
  padding: 12px 20px;
  font-size: 16px;
  border-radius: 12px;
  border: 1px solid #f0b3c1;
  background-color: #fff;
  color: #6a4c92;
  transition: 0.3s ease;
}

select:hover {
  border-color: #f1778a;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  padding: 16px;
  background: #fff;
  margin-bottom: 14px;
  border-radius: 14px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  transition: background 0.3s ease, transform 0.3s ease;
}

li:hover {
  background: #f8e6f7;
  transform: translateY(-4px);
}

li.done span {
  text-decoration: line-through;
  color: #aaa;
}

li span {
  flex: 1;
  margin-left: 16px;
  font-size: 17px;
  color: #555;
}

.delete {
  background: transparent;
  color: #e74c3c;
  font-size: 16px;
  border: none;
  cursor: pointer;
  transition: 0.3s ease;
}

.delete:hover {
  color: #c0392b;
}

.footer {
  margin-top: 40px;
  border-top: 1px solid #eee;
  padding-top: 14px;
  text-align: center;
  font-size: 14px;
  color: #aaa;
  font-style: italic;
}
</style>