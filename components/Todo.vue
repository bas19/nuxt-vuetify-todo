<script setup>
import { ref, watch } from 'vue'
const showSaveBtn = ref(false)
const newTodo = ref(null)

const props = defineProps({
  id: Number,
  todo: String,
  is_done: Boolean,
})
const emit = defineEmits(["updateTodo", "deleteTodo", "doneTodo"]);

const editTodo = () => {
  showSaveBtn.value = !showSaveBtn.value
  emit('updateTodo', {
    id: props.id,
    todo: newTodo.value,
    is_done: props.is_done,
  })
}

const checkTodo = (value) => {
  newTodo.value = value
  if (value != props.todo.value ) {
    showSaveBtn.value = true
  }
}

const doneTodo = (data) => {
  emit('doneTodo', {
    id: props.id,
    todo: props.todo,
    is_done: !props.is_done,
  })
}

const deleteTodo = () => {
  emit('deleteTodo', props.id)
}

</script>

<template>
  <div class="wrapper">
    <v-text-field
      type="text"
      :model-value="todo"
      @click:clear="editTodo"
      @keyup="checkTodo($event.target.value)"
      :key="id"
      clearable
      outline
    >
      <template #prepend>
        <v-checkbox
          class="mt-6"
          :key="props.id"
          :model-value="props.is_done ? true : false"
          @click="doneTodo(props.id, props.todo, props.is_done)"
        ></v-checkbox>
      </template>
      <template #append>
        <v-btn
          size="small"
          color="secondary"
          @click="editTodo"
          v-if="showSaveBtn"
        >
          Save
        </v-btn>
        <v-btn
            prepend-icon="mdi-trash-can"
            @click="deleteTodo"
            v-if="!showSaveBtn"
            class="ml-2"
            type="submit"
            size="small"
            variant="flat"
          >
            <template v-slot:prepend>
              <v-icon color="red"></v-icon>
            </template>
            Delete
          </v-btn>
      </template>
    </v-text-field>
  </div>
</template>