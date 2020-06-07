<template>
  <div>
    <div class="flex-space-between">
      <h1>할 일</h1>
      <button class="toggle-button" @click="showCompleted = !showCompleted">
        {{ toggleButtonText }}
      </button>
    </div>
    <div class="scrollable">
      <Draggable
        v-model="notCompletedTodos"
        group="people"
        @start="drag = true"
        @end="drag = false"
      >
        <TodoListItem
          v-for="todo in notCompletedTodos"
          :key="todo.id"
          :todo="todo"
          @remove-todo="removeTodo"
          @update-todo="updateTodo"
          @toggle-is-completed="toggleIsCompleted"
        />
      </Draggable>

      <div v-if="showCompleted">
        <span class="separating-span">완료됨</span>
        <TodoListItem
          v-for="todo in completedTodos"
          :key="todo.id"
          :todo="todo"
          @remove-todo="removeTodo"
          @update-todo="updateTodo"
          @toggle-is-completed="toggleIsCompleted"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TodoListItem from './TodoListItem'
import Draggable from 'vuedraggable'

export default {
  components: {
    TodoListItem,
    Draggable,
  },
  props: {
    todos: Array,
  },
  data() {
    return {
      showCompleted: false,
    }
  },
  computed: {
    notCompletedTodos() {
      return this.todos.filter((todo) => !todo.isCompleted)
    },
    completedTodos() {
      return this.todos.filter((todo) => todo.isCompleted)
    },
    toggleButtonText() {
      return this.showCompleted ? '완료된 작업 숨기기' : '완료된 작업 표시'
    },
  },
  methods: {
    removeTodo(id) {
      this.$emit('remove-todo', id)
    },
    updateTodo(id, content) {
      this.$emit('update-todo', id, content)
    },
    toggleIsCompleted(todo) {
      this.$emit('toggle-is-completed', todo)
    },
  },
}
</script>

<style>
.separating-span {
  background-color: rgb(92, 96, 103);
  color: white;
  border-radius: 10px;
  padding: 2px 10px;
}
.scrollable {
  overflow: overlay;
  height: 78vh;
  padding: 0px 15px;
}
::-webkit-scrollbar {
  width: 10px;
}
::-webkit-scrollbar-thumb {
  border-radius: 5px;
  background: rgba(90, 90, 90);
}

::-webkit-scrollbar-track {
  border-radius: 5px;
  background: rgba(0, 0, 0, 0.2);
}
.flex-space-between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.toggle-button {
  background-color: rgb(50, 50, 50);
  border: none;
  border-radius: 10px;
  color: white;
  width: 120px;
  height: 30px;
}
.toggle-button:focus {
  outline: none;
}
</style>
