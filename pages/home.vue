<script setup>
import Todo from '../components/Todo.vue'

const newTodo = ref(null);
const items = ref([
  { id: 1, todo: 'Todo 1', done: false },
  { id: 2, todo: 'Todo 2', done: false },
  { id: 3, todo: 'Todo 3', done: false },
  { id: 4, todo: 'Todo 4', done: false },
  { id: 5, todo: 'Todo 5', done: false },
])

const addTodo = () => {
  let data = items.value.slice();
  data.unshift({
    id: ++items.value.length,
    todo: newTodo.value,
    done: false,
  })
  items.value = data;
}

const updateTodo = (data) => {
  console.log(data);
  // items.value[index].name = 'Test 1'
  // call API endpoint to update
}

const deleteTodo = (index) => {
  items.value.splice(index, 1)
}
</script>

<template>
  <div>
    <v-container>
      <v-sheet class="mx-auto pa-4" width="500">
        <v-text-field
          label="Add new todo"
          type="text"
          v-model="newTodo"
          clearable
        >
          <template #append>
            <v-btn
              color="secondary"
              type="submit"
              size="large"
              outlined
              @click="addTodo"
            >
              Add
            </v-btn>
          </template>
        </v-text-field>
        <v-divider></v-divider>
        <h1 class="my-8">Tasks</h1>
        <div>
          <Todo
            v-for="(item, index) in items" :key="index"
            :todo="item.todo"
            :done="item.done"
            v-model="item.todo"
            @updateTodo="updateTodo"
            @deleteTodo="deleteTodo(index)"
          />
        </div>
      </v-sheet>
    </v-container>
  </div>
</template>

<style scoped>
</style>