<script setup>
import TodoItem from './components/TodoItem.vue'
import PlusIcon from '@/components/icons/PlusIcon.vue'
import { onMounted, ref } from 'vue'
import axios from 'axios'

const responseData = ref(null)
const taskInput = ref('')

onMounted(async () => {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/todos')
    responseData.value = response.data
  } catch (error) {
    console.error('Error:', error)
  }
})

const handleEditTodo = (newTitle, todoIndex) => {
  const updatedResponseData = [...responseData.value]
  updatedResponseData[todoIndex] = { ...updatedResponseData[todoIndex], title: newTitle }
  responseData.value = updatedResponseData
}

const handleDeleteTodo = (todoIndex) => {
  const updatedResponseData = [...responseData.value]
  updatedResponseData.splice(todoIndex, 1)
  responseData.value = updatedResponseData
}

const handleCheckTodo = (todoIndex, isCompleted) => {
  const updatedResponseData = [...responseData.value]
  updatedResponseData[todoIndex] = { ...updatedResponseData[todoIndex], completed: isCompleted }
  responseData.value = updatedResponseData
}

const AddTodo = () => {
  if (taskInput.value.trim() !== '') {
    const newTask = {
      title: taskInput.value,
      completed: false
    }
    responseData.value.unshift(newTask)
    taskInput.value = ''
  }
}
</script>

<template>
  <main>
    <div v-if="responseData">
      <div class="headerdiv">
        <h2 style="white-space: nowrap">Todo Tasks:</h2>
        <form class="newtaskdiv" @submit.prevent="AddTodo">
          <label class="labelstyles">New Task</label>
          <input class="inputstyles" v-model="taskInput" />
          <button class="plusbutton">
            <PlusIcon class="svgsizes"></PlusIcon>
          </button>
        </form>
      </div>
      <TodoItem
        v-for="(item, index) in responseData"
        :key="index"
        :todo="item"
        :todoid="index"
        @edit-todo="handleEditTodo"
        @delete-todo="handleDeleteTodo"
        @check-todo="handleCheckTodo"
      ></TodoItem>
    </div>
  </main>
</template>
