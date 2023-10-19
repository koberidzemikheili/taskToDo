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
</script>

<template>
  <main>
    <button @click="getData">Fetch Data</button>
    <div v-if="responseData">
      <h2>Response Data:</h2>
      <TodoItem v-for="item in responseData" :key="item.id" :todo="item"></TodoItem>
    </div>
  </main>
</template>
