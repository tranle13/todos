<script setup lang="ts">
import TodoCreator from '@/components/TodoCreator.vue';
import TodoItem from '@/components/TodoItem.vue';
import { Icon } from "@iconify/vue";
import { uid } from 'uid';
import { ref } from 'vue';

export interface Todo {
  id: string;
  todo: string;
  isCompleted: boolean | null;
  isEditing: boolean | null;
}

const todoList = ref<Todo[]>([])
const createTodo = (todo: string) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null
  })
}
const editTodo = (id: string) => {
  todoList.value.map(todo => {
    if (todo.id === id) todo.isEditing = todo.isEditing ? null : true;
    return todo;
  })
}
const toggleComplete = (id: string) => {
  todoList.value.map(todo => {
    if (todo.id === id) todo.isCompleted = todo.isCompleted ? null : true
    return todo;
  })
}
const doneEditTodo = (id: string, newTodo: string) => {
  todoList.value.map(todo => {
    if (todo.id === id) {
      todo.todo = newTodo;
      todo.isEditing = null;
    }
    return todo;
  })
}
const deleteTodo = (id: string) => {
  todoList.value = todoList.value.filter(todo => todo.id !== id)}
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo"/>
    <ul v-if="todoList.length > 0" class="todo-list">
      <TodoItem v-for="todo in todoList" :key="todo.id" :todo="todo" @edit-todo="editTodo" @toggle-complete="toggleComplete" @done-edit-todo="doneEditTodo" @delete-todo="deleteTodo"/>
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" width="22"/>
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
