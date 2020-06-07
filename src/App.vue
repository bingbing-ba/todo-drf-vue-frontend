<template>
  <div
    class="container"
    :style="{ backgroundImage: `url(${backgroundImage})` }"
  >
    <TodoList
      :todos="todos"
      @remove-todo="removeTodo"
      @update-todo="updateTodo"
      @toggle-is-completed="toggleIsCompleted"
    />
    <TodoInput @add-new-todo="addNewTodo" />
  </div>
</template>

<script>
import axios from 'axios'
import TodoInput from './components/TodoInput'
import TodoList from './components/TodoList'
import beachBackground from './assets/beach_background.jpg'

const API_BASE = 'https://bingbingba-todo-drf.herokuapp.com/api/v1/todo/'

export default {
  components: {
    TodoList,
    TodoInput,
  },
  data() {
    return {
      todos: [],
    }
  },
  computed: {
    backgroundImage() {
      return beachBackground
    },
  },
  methods: {
    async fetchTodos() {
      const { data } = await axios.get(API_BASE)
      this.todos = data.todos
    },
    async addNewTodo(newTodo) {
      const { data } = await axios.post(API_BASE, {
        content: newTodo,
      })
      if (data.status === 'failure') {
        alert('요청이 실패했습니다 개발자에게 문의하세여')
      } else {
        this.fetchTodos()
      }
    },
    async removeTodo(id) {
      const { data } = await axios.delete(`${API_BASE}${id}`)
      if (data.status === 'success') {
        this.fetchTodos()
      } else {
        alert('삭제가 완료되지 않았습니다. 관리자에게 문의하세여')
      }
    },
    async updateTodo(id, content) {
      const { data } = await axios.patch(`${API_BASE}${id}`, { content })
      if (data.status === 'success') {
        this.fetchTodos()
      } else {
        alert('수정이 완료되지 않았습니다. 관리자에게 문의하세여')
      }
    },
    async toggleIsCompleted(todo) {
      const { data } = await axios.patch(`${API_BASE}${todo.id}`, {
        isCompleted: !todo.isCompleted,
      })
      if (data.status === 'success') {
        this.fetchTodos()
      } else {
        alert('수정이 완료되지 않았습니다. 관리자에게 문의하세여')
      }
    },
  },
  created() {
    this.fetchTodos()
  },
}
</script>

<style>
* {
  box-sizing: border-box;
  overflow: hidden;
  margin: 0;
  padding: 0;
}
.container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
  height: 100vh;
  color: white;
  padding-top: 3vh;
  padding-left: 10vh;
  padding-right: 10vh;
  background-size: cover;
}
</style>
