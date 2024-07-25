<script setup>
import { ref, watch } from 'vue'
const showSaveBtn = ref(false)
const props = defineProps({
  id: Number,
  todo: String,
  done: Boolean,
})
const emit = defineEmits(["updateTodo", "deleteTodo"]);
const updateTodo = (value) => {
  if (value != props.todo.value ) {
    showSaveBtn.value = true
  }
  emit('updateTodo', {
    id: props.id,
    todo: value
  })
}

const editTodo = () => {
  showSaveBtn.value = !showSaveBtn;
  // emit('editTodo', props.id)
}

const deleteTodo = () => {
  emit('deleteTodo', props.id)
}

</script>

<template>
  <div class="wrapper">
    <v-text-field
      type="text"
      clearable
      :model-value="todo"
      variant="outlined"
      @click:clear="editTodo"
      @keyup="updateTodo($event.target.value)"
    >
      <template #append>
        <v-btn
          size="small"
          color="primary"
          v-if="showSaveBtn"
        >
          Save
        </v-btn>
        <!-- <v-btn
          size="small"
          variant="text"
          v-if="!edited"
        >
          Done
        </v-btn> -->
        <v-btn
          class="ml-2"
          color="red"
          type="submit"
          size="small"
          @click="deleteTodo"
          v-if="!showSaveBtn"
        >
          Delete
        </v-btn>
      </template>
    </v-text-field>
  </div>
</template>