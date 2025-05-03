<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="addTodo">
      <input v-model="newTodo" placeholder="Masukkan kegiatan..." />
      <button type="submit">Tambah</button>
    </form>

    <select v-model="filter">
      <option value="all">Semua</option>
      <option value="pending">Belum Selesai</option>
      <option value="completed">Sudah Selesai</option>
    </select>

    <ul>
      <li
        v-for="(todo, index) in filteredTodos"
        :key="index"
        :class="{ done: todo.completed }"
      >
        <input type="checkbox" v-model="todo.completed" />
        <span>{{ todo.text }}</span>
        <button @click="removeTodo(index)">Hapus</button>
      </li>
    </ul>

    <footer>
      &copy; {{ new Date().getFullYear() }} Sahira Ayiska
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const filter = ref('all')

const todos = ref([
  { text: 'Belajar matematika', completed: false },
  { text: 'Berbelanja', completed: false },
  { text: 'Melukis', completed: true }
])

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value.trim(), completed: false })
    newTodo.value = ''
  }
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  if (filter.value === 'pending') return todos.value.filter(t => !t.completed)
  if (filter.value === 'completed') return todos.value.filter(t => t.completed)
  return todos.value
})
</script>

<style scoped>
.container {
  max-width: 500px;
  margin: 30px auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

form {
  display: flex;
  gap: 10px;
  margin-bottom: 16px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
  font-size: 16px;
}

button {
  padding: 8px 12px;
  background-color: #f1778a;
  color: white;
  border: none;
  cursor: pointer;
}

select {
  width: 100%;
  padding: 8px;
  margin-bottom: 16px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  padding: 8px;
  border: 1px solid #ddd;
}

li.done span {
  text-decoration: line-through;
  color: #777;
}

li span {
  flex: 1;
  margin-left: 10px;
}

footer {
  text-align: center;
  margin-top: 20px;
  font-size: 12px;
  color: #888;
}
</style>