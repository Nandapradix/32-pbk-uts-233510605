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