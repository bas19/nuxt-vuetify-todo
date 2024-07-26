<script setup>
import Todo from '../components/Todo.vue'

const new_todo = ref(null);
const user_id = ref(null);

const items = ref([])

const addTodo = async () => {
  try {
    const input = { todo: new_todo.value, user: { connect: user_id.value } }
    const result = await GqlCreateTodoMutation({input: input})
    if ( result ) {
      new_todo.value = null;
      let data = items.value.slice();
      data.unshift({
        id: result.createTodo.id,
        todo: result.createTodo.todo,
        done: false,
      })
      items.value = data;
    }
  } catch(e) {
    console.log(e)
  }
}

const updateTodo = async (data) => {
  const update_todo = await GqlUpdateTodoMutation({
    id: data.id,
    todo: data.todo
  })
}

const doneTodo = async (data) => {
  const update_todo = await GqlUpdateTodoMutation({
    id: data.id,
    todo: data.todo,
    is_done: data.is_done ? 1 : 0,
  })
}

const deleteTodo = async (index, id) => {
  items.value.splice(index, 1)
  const deleted_todo = await GqlDeleteTodoMutation({
    id: id
  })
  console.log(deleted_todo)
}

const getUserInfo = async () => {
  const user = await GqlGetUser()
  console.log('user data', user)
  user_id.value = user.me.id
  items.value = user.me.todos
  localStorage.setItem('user_id', user.me.id)
  localStorage.setItem('user_name', user.me.name)
  localStorage.setItem('user_email', user.me.email)
}

onMounted(() => {
  getUserInfo()
})

</script>

<template>
  <div>
    <v-container>
      <v-sheet class="mx-auto pa-4" width="800">
        <v-text-field
          label="Add new todo"
          type="text"
          v-model="new_todo"
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
            :id="item.id"
            :todo="item.todo"
            :is_done="item.is_done"
            v-model="item.todo"
            @doneTodo="doneTodo"
            @updateTodo="updateTodo"
            @deleteTodo="deleteTodo(index, item.id)"
          />
        </div>
      </v-sheet>
    </v-container>
  </div>
</template>

<style scoped>
</style>