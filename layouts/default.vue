<script setup>

const user_name = ref(null);
const user_email = ref(null);

const logout = async () => {
  await GqlLogoutMutation
  localStorage.clear();
  useGqlToken(null)
  navigateTo('/')
}

onBeforeMount(() => {
  user_name.value = localStorage.getItem('user_name')
  user_email.value = localStorage.getItem('user_email')
})

</script>
<template>
  <div>
    <v-app>
      <v-navigation-drawer
        expand-on-hover
        rail
      >
        <v-list>
          <v-list-item
            :subtitle="user_email"
            :title="user_name"
            prepend-icon="mdi-account"
          />
        </v-list>
      </v-navigation-drawer>
      <v-app-bar 
        title="Todo App" 
        color="teal"
      >
        <template v-slot:append>
          <v-btn density="default" @click="logout()">Logout</v-btn>
        </template>
      </v-app-bar>
      <v-main>
        <slot />
      </v-main>
    </v-app>
  </div>
</template>