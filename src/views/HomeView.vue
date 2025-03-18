<template>
  <main>
    <section class="todo-app">
      <h1>To-Do List</h1>
      <form @submit.prevent="addTodo">
        <input type="text" v-model="newTodo" placeholder="Add a new task" maxlength="255" />
        <button type="submit">Add</button>
      </form>
      <ul>
        <li v-for="(todo, index) in todos" :key="index">
          <input type="checkbox" v-model="todo.completed" />
          <span :class="{ completed: todo.completed }" class="todo-text">{{ todo.text }}</span>
          <button @click="removeTodo(index)">Remove</button>
        </li>
      </ul>
    </section>
  </main>
</template>

<script>
export default {
  name: 'HomeView',
  data() {
    return {
      url: 'http://127.0.0.1:8000',
      newTodo: '',
      todos: [],
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({ text: this.newTodo, completed: false })
        this.newTodo = ''
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
    },
  },
}
</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  padding: 1rem;
  margin-top: 60px;
  flex: 1;
}

.todo-app {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 1rem;
}

form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 1rem;
}

input[type='text'] {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 0.5rem 1rem;
  border: none;
  background-color: #007bff;
  color: #fff;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 0;
  border-bottom: 1px solid #eee;
}

li:last-child {
  border-bottom: none;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

input[type='checkbox'] {
  margin-right: 0.5rem;
}

.todo-text {
  flex: 1;
  white-space: normal;
  word-wrap: break-word;
  margin-right: 1rem;
  max-width: calc(100% - 100px);
}

/* Media queries for responsiveness */
@media (min-width: 600px) {
  form {
    flex-direction: row;
  }

  input[type='text'] {
    flex: 1;
  }

  .todo-app {
    width: 600px;
  }
}
</style>
