<script setup>
import TodoItem from './components/TodoItem.vue'
import { ref } from 'vue'
import axios from 'axios'

const responseData = ref(null)

const getData = async () => {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/todos')
    responseData.value = response.data
  } catch (error) {
    console.error('Error:', error)
  }
}

const handleEditTodo = (newTitle, todoId) => {
  const updatedResponseData = responseData.value.map((item) => {
    if (item.id === todoId) {
      return { ...item, title: newTitle }
    }
    return item
  })

  responseData.value = updatedResponseData
}

const handleDeleteTodo = (todoId) => {
  responseData.value = responseData.value.filter((item) => item.id !== todoId)
}
const handleCheckTodo = (todoId, isCompleted) => {
  const updatedResponseData = responseData.value.map((item) => {
    if (item.id === todoId) {
      return { ...item, completed: isCompleted }
    }
    return item
  })

  responseData.value = updatedResponseData
}
</script>

<template>
  <main>
    <button @click="getData">Fetch Data</button>
    <div v-if="responseData">
      <h2>Todo Tasks:</h2>
      <TodoItem
        v-for="item in responseData"
        :key="item.id"
        :todo="item"
        @edit-todo="handleEditTodo"
        @delete-todo="handleDeleteTodo"
        @check-todo="handleCheckTodo"
      ></TodoItem>
    </div>
  </main>
</template>
