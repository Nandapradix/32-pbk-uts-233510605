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