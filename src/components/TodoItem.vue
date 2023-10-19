<template>
  <div class="itemstyle">
    <p @dblclick="startEditing">{{ editing ? '' : todo.title }}</p>
    <input
      class="inputstyle"
      v-if="editing"
      v-model="editedText"
      @blur="stopEditing"
      @keydown.enter="stopEditing"
    />
    <EditIcon class="svgsize editbutton" @click="startEditing"></EditIcon>
    <DeleteIcon class="svgsize editbutton"></DeleteIcon>
  </div>
</template>

<script setup>
import EditIcon from '@/components/icons/EditIcon.vue'
import DeleteIcon from '@/components/icons/DeleteIcon.vue'
import { ref } from 'vue'

const props = defineProps({
  todo: Object
})

const editing = ref(false)
const editedText = ref(props.todo.title)

const startEditing = () => {
  editing.value = editing.value ? false : true
}

const stopEditing = () => {
  editing.value = false
}
</script>
