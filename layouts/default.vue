<script setup>

const logout = async () => {
  await GqlLogoutMutation
  localStorage.clear();
  useGqlToken(null)
  navigateTo('/')
}
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