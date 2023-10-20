<script setup>
import EditIcon from '@/components/icons/EditIcon.vue'
import DeleteIcon from '@/components/icons/DeleteIcon.vue'
import { ref, watch } from 'vue'

const props = defineProps({
  todo: Object
})

const emit = defineEmits(['edit-todo', 'delete-todo', 'check-todo'])

const editing = ref(false)
const editedText = ref(props.todo.title)
const isCompleted = ref(props.todo.completed)

const startEditing = () => {
  editing.value = editing.value ? false : true
}

const stopEditing = () => {
  editing.value = false
}

const saveTitle = () => {
  editing.value = false
  emit('edit-todo', editedText.value, props.todo.id)
}

const deleteItem = () => {
  emit('delete-todo', props.todo.id)
}

watch(isCompleted, (newValue) => {
  emit('check-todo', props.todo.id, newValue)
})
</script>

<template>
  <div class="itemstyle">
    <p @dblclick="startEditing" :class="{ 'completed-task': isCompleted }">
      {{ editing ? '' : todo.title }}
    </p>
    <input
      class="inputstyle"
      v-if="editing"
      v-model="editedText"
      @blur="stopEditing"
      @keydown.enter="saveTitle"
    />
    <label>
      <input class="checkboxstyles" type="checkbox" v-model="isCompleted" />
    </label>
    <EditIcon class="svgsize editbutton" @click="startEditing"></EditIcon>
    <DeleteIcon class="svgsize editbutton" @click="deleteItem"></DeleteIcon>
  </div>
</template>
