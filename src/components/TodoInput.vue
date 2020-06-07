<template>
  <div class="bottom-fixed">
    <div class="todo-input-box">
      <i class="btn" :class="focusedClass"></i>
      <input
        type="text"
        placeholder="작업 추가"
        class="todo-input"
        v-model="newTodo"
        @keypress.enter="addNewTodo"
        @focus="isFocused = true"
        @blur="isFocused = false"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      isFocused: false,
    }
  },
  computed: {
    focusedClass() {
      return this.isFocused ? 'far fa-circle' : 'fas fa-plus'
    },
  },
  methods: {
    addNewTodo() {
      if (this.newTodo.trim()) {
        this.$emit('add-new-todo', this.newTodo.trim())
        this.newTodo = ''
      } else {
        // 진동 애니메이션
        alert('올바로 입력하세여')
      }
    },
  },
}
</script>

<style>
.bottom-fixed {
  position: fixed;
  background-color: transparent;
  height: 12vh;
  width: 80vw;
  bottom: 0px;
}
.todo-input-box {
  display: flex;
  align-items: center;
  border-radius: 5px;
  background-color: rgb(50, 50, 50);
  margin: 10px 0;
  padding: 10px 10px;
  cursor: grab;
}
.todo-input {
  margin-left: 10px;
  border: none;
  background-color: transparent;
  color: white;
  font-size: 1rem;
}
.todo-input:focus {
  outline: none;
}
::placeholder {
  color: white;
}
</style>
