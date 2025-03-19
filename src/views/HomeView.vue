<template>
  <main>
    <section class="todo-app">
      <h1>To-Do List</h1>
      <form @submit.prevent="addTodo">
        <input type="text" v-model="newTask" placeholder="Add a new task" maxlength="255" />
        <button class="btn-primary" type="submit">Add</button>
      </form>
      <ul>
        <li v-for="(todo, index) in todos" :key="todo.id_task">
          <input type="checkbox" v-model="todo.completed" @change="updateTodoStatus(todo)" />
          <span :class="{ completed: todo.completed }" class="todo-text">{{ todo.task }}</span>
          <button class="btn-danger" @click="removeTodo(todo.id_task, index)">Remove</button>
        </li>
      </ul>
      <div v-if="isLoading" class="overlay"></div>
    </section>
  </main>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HomeView',
  data() {
    return {
      isLoading: false,
      url: 'http://127.0.0.1:8000/api',
      newTask: '',
      todos: [],
    }
  },
  methods: {
    // Fetch todos tasks from the API
    async fetchTodos() {
      this.isLoading = true
      try {
        const response = await axios.get(`${this.url}/tasks`)
        this.todos = response.data.map((task) => ({
          id_task: task.id_task,
          task: task.task,
          completed: task.completed === 1,
          completed_at: task.completed_at,
          created_at: task.created_at,
          updated_at: task.updated_at,
        }))
      } catch (error) {
        console.error('Error fetching todos:', error)
      } finally {
        this.isLoading = false
      }
    },
    // Add a new todo
    async addTodo() {
      if (this.newTask.trim() !== '') {
        this.isLoading = true
        try {
          const response = await axios.post(`${this.url}/tasks`, {
            task: this.newTask,
          })
          this.todos.push(response.data.item)
          this.newTask = ''
        } catch (error) {
          console.error('Error adding todo:', error)
        } finally {
          this.isLoading = false
        }
      }
    },
    // Update the completion status of a todo
    async updateTodoStatus(todo) {
      this.isLoading = true
      try {
        await axios.put(`${this.url}/tasks/${todo.id_task}`, {
          completed: todo.completed,
        })
      } catch (error) {
        console.error('Error updating todo completion status:', error)
      } finally {
        this.isLoading = false
      }
    },
    // Remove a todo
    async removeTodo(id_task, index) {
      this.isLoading = true
      try {
        await axios.delete(`${this.url}/tasks/${id_task}`)
        this.todos.splice(index, 1)
      } catch (error) {
        console.error('Error removing todo:', error)
      } finally {
        this.isLoading = false
      }
    },
  },
  mounted() {
    this.fetchTodos()
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
  color: #fff;
  border-radius: 4px;
  cursor: pointer;
}

.btn-primary {
  background-color: #007bff;
}

.btn-primary:hover {
  background-color: #0069d9;
}

.btn-danger {
  background-color: #dc3545;
}

.btn-danger:hover {
  background-color: #c82333;
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
  padding-right: 1rem;
  max-width: calc(100% - 100px);
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1000;
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
