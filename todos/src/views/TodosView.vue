<script setup lang="ts">
import TodoCreator from '@/components/TodoCreator.vue'
import TodoItem from '@/components/TodoItem.vue'
import { Icon } from '@iconify/vue'
import { uid } from 'uid'
import { ref } from 'vue'

export interface Todo {
  id: string
  todo: string
  isCompleted: boolean
  isEditing: boolean
}

const todoList = ref<Todo[]>([])

const fetchTodoList = () => {
  const savedTodoList = localStorage.getItem('todoList')
  if (savedTodoList) {
    todoList.value = JSON.parse(savedTodoList) as Todo[]
  }
}

fetchTodoList()

const setTodoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value))
}
const createTodo = (todo: string) => {
  console.log(typeof todoList.value)

  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: false
  })
  setTodoListLocalStorage()
}
const toggleEditTodo = (index: number) => {
  todoList.value[index].isEditing = !todoList.value[index].isEditing
  setTodoListLocalStorage()
}
const toggleTodoComplete = (index: number) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted
  setTodoListLocalStorage()
}
const updateTodo = (newTodo: string, index: number) => {
  todoList.value[index].todo = newTodo
  setTodoListLocalStorage()
}
const deleteTodo = (id: string) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== id)
  setTodoListLocalStorage()
}
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul v-if="todoList.length > 0" class="todo-list">
      <TodoItem
        v-for="(todo, index) in todoList"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
