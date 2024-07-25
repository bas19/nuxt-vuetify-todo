<template>
  <div>
    <v-app>
      <v-app-bar 
        title="Todo App" 
        color="teal"
      >
    </v-app-bar>
      <v-main>
        <slot />
      </v-main>
    </v-app>
  </div>
</template>