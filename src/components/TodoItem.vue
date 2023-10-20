<script setup>
import EditIcon from '@/components/icons/EditIcon.vue'
import DeleteIcon from '@/components/icons/DeleteIcon.vue'
import { ref, watch } from 'vue'

const props = defineProps({
  todo: Object,
  todoid: Number
})

const emit = defineEmits(['edit-todo', 'delete-todo', 'check-todo'])

const editing = ref(false)
const editedText = ref('')
const isCompleted = ref(props.todo.completed)

const startEditing = () => {
  editing.value = editing.value ? false : true
  editedText.value = props.todo.title
}

const stopEditing = () => {
  editing.value = false
}

const saveTitle = () => {
  editing.value = false
  emit('edit-todo', editedText.value, props.todoid)
}

const deleteItem = () => {
  emit('delete-todo', props.todoid)
}

watch(isCompleted, (newValue) => {
  emit('check-todo', props.todoid, newValue)
})
</script>

<template>
  <div class="itemstyle">
    <label>
      <input class="checkboxstyles" type="checkbox" v-model="isCompleted" />
    </label>
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
    <EditIcon class="svgsize editbutton" @click="startEditing"></EditIcon>
    <DeleteIcon class="svgsize editbutton" @click="deleteItem"></DeleteIcon>
  </div>
</template>
