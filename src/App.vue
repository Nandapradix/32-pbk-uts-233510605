<template>
  <div id="app" :class="{ dark: isDarkMode }">
    <button class="theme-toggle" @click="toggleTheme">
      {{ isDarkMode ? '🌙 Gelap' : '☀️ Terang' }}
    </button>

    <h1>To-Do List</h1>

    <input
      v-model="newTodo"
      @keyup.enter="addTodo"
      placeholder="Tambahkan kegiatan baru..."
    />

    <label class="filter">
      <input type="checkbox" v-model="showIncompleteOnly" />
      Tampilkan hanya tugas yang belum selesai 
    </label>

    <div class="controls">
      <button @click="markAllDone">Tandai Semua Selesai</button>
      <button @click="clearCompleted">Hapus Semua Selesai</button>
    </div>

    <ul>
      <li
        v-for="(todo, index) in filteredTodos"
        :key="index"
        :class="{ completed: todo.done }"
      >
        <input type="checkbox" v-model="todo.done" />
        <span v-if="!todo.editing">{{ todo.text }}</span>
        <input
          v-else
          v-model="todo.text"
          @keyup.enter="finishEdit(todo)"
          @blur="finishEdit(todo)"
        />
        <button @click="editTodo(todo)" v-if="!todo.editing">Edit</button>
        <button @click="hapusTodo(index)">Hapus</button>
      </li>
    </ul>

    <p class="stats">
      Total: {{ todos.length }} |
      Belum selesai: {{ todos.filter(todo => !todo.done).length }}
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      showIncompleteOnly: false,
      isDarkMode: false,
    };
  },
  computed: {
    filteredTodos() {
      return this.showIncompleteOnly
        ? this.todos.filter((todo) => !todo.done)
        : this.todos;
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({ text: this.newTodo.trim(), done: false, editing: false });
        this.newTodo = '';
      }
    },
    hapusTodo(index) {
      this.todos.splice(index, 1);
    },
    markAllDone() {
      this.todos.forEach((todo) => {
        todo.done = true;
      });
    },
    clearCompleted() {
      this.todos = this.todos.filter((todo) => !todo.done);
    },
    editTodo(todo) {
      todo.editing = true;
    },
    finishEdit(todo) {
      todo.editing = false;
    },
    toggleTheme() {
      this.isDarkMode = !this.isDarkMode;
    },
  },
};
</script>

<style scoped>
html, body {
  margin: 0;
  padding: 0;
  width: 100vw;
  height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #f5f7fa;
}

#app {
  width: 100%;
  height: 100%;
  padding: 40px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  background: #ffffff;
  overflow-y: auto;
  position: relative;
}

h1 {
  text-align: center;
  color: #333;
  font-size: 32px;
  margin-bottom: 30px;
}

input[type='text'] {
  width: 100%;
  padding: 18px;
  border: 2px solid #007bff;
  border-radius: 10px;
  margin-bottom: 20px;
  font-size: 20px;
  box-sizing: border-box;
  height: 60px;
}

input[type='checkbox'] {
  transform: scale(1.2);
  margin-right: 10px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  background: #000000;
  color: white;
  padding: 12px 18px;
  border-radius: 8px;
  margin-bottom: 12px;
  transition: background 0.2s ease;
}

li:hover {
  background: #2e3235;
}

.completed {
  text-decoration: line-through;
  color: #999;
}

button {
  margin-left: 10px;
  background-color: #e74c3c;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

button:hover {
  background-color: #c0392b;
}

.filter {
  display: flex;
  align-items: center;
  font-size: 16px;
  margin-bottom: 20px;
  color: #444;
}

.controls {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-bottom: 25px;
}

.controls button {
  flex: 1;
  background-color: #3498db;
  padding: 10px 14px;
  font-size: 16px;
}

.controls button:last-child {
  background-color: #9b59b6;
}

.stats {
  text-align: center;
  margin-top: auto;
  font-size: 15px;
  color: #555;
}

.theme-toggle {
  position: absolute;
  top: 20px;
  right: 20px;
  padding: 8px 12px;
  border: none;
  border-radius: 6px;
  background: #f1c40f;
  color: #000;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.3s ease;
  z-index: 10;
}

.theme-toggle:hover {
  background: #d4ac0d;
}

.dark {
  background-color: #1e1e1e;
  color: #f1f1f1;
}

.dark li {
  background: #2c2c2c;
  color: #f1f1f1;
}

.dark li:hover {
  background: #3a3a3a;
}

.dark input[type='text'] {
  background-color: #333;
  color: white;
  border-color: #555;
}

.dark .filter,
.dark .stats {
  color: #ccc;
}

.dark .controls button {
  background-color: #555;
}

.dark .controls button:hover {
  background-color: #666;
}
</style>
