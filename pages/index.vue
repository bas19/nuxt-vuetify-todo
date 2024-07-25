<script setup>
  definePageMeta({
    layout: false,
  })

  const register = () => {
    navigateTo('/register')
  }
</script>

<template>
  <div>
    <NuxtLayout name="auth">
      <v-sheet class="mx-auto pa-4 mt-12" width="600">
        <v-form ref="form">
          <v-text-field
            v-model="email"
            :counter="10"
            :rules="nameRules"
            label="Email"
            required
          ></v-text-field>

          <v-text-field
            v-model="password"
            :readonly="loading"
            :rules="[required]"
            label="Password"
            placeholder="Enter your password"
            clearable
            type="password"
          ></v-text-field>

          <div class="d-flex flex-column">
            <v-btn
              class="mt-4"
              color="secondary"
              block
              @click="validate"
            >
              Login
            </v-btn>

            <v-btn
              class="mt-4"
              block
              @click="register"
            >
              Create Account
            </v-btn>
          </div>
         </v-form>
        </v-sheet>
    </NuxtLayout>
  </div>
</template>