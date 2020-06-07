<template>
  <div class="todo-input-box flex-space-between">
    <div class="flex align-center">
      <i :class="checkButtonClass" @click="toggleIsCompleted" class="btn"></i>
      <input
        ref="todoInput"
        type="text"
        class="todo-input"
        :value="todo.content"
        @keypress.enter="updateTodo"
        @blur="setOriginContent"
      />
    </div>

    <i class="far fa-trash-alt btn remove-btn" @click="removeTodo"></i>
  </div>
</template>

<script>
export default {
  props: {
    todo: Object,
  },
  computed: {
    checkButtonClass() {
      return this.todo.isCompleted ? 'fas fa-check-circle' : 'far fa-circle'
    },
  },
  methods: {
    removeTodo() {
      this.$emit('remove-todo', this.todo.id)
    },
    updateTodo(event) {
      const newTodo = event.target.value.trim()
      if (!newTodo) {
        // 진동 애니메이션
        alert('값이 비었어여')
      } else {
        this.$emit('update-todo', this.todo.id, newTodo)
        this.$refs.todoInput.blur()
      }
    },
    setOriginContent(event) {
      event.target.value = this.todo.content
    },
    toggleIsCompleted() {
      this.$emit('toggle-is-completed', this.todo)
    },
  },
}
</script>

<style>
.btn {
  cursor: pointer;
  color: rgb(147, 147, 147);
}
.remove-btn {
  margin-left: 10px;
}
.flex {
  display: flex;
}
.align-center {
  align-items: center;
}
</style>
