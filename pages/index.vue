<script setup>
  definePageMeta({
    layout: false,
  })

  const email = defineModel('email')
  const password = defineModel('password')

  const handleLogin = async () => {
    if (!email.value || !password.value) {
      alert('Please input email/password')
      return
    }
    const variables = { 
      email: email.value,
      password: password.value,
    }
    try {
      const result = await GqlLoginMutation(variables)
      console.log(result)
      if ( result.login === 'Invalid') {
        alert('Invalid email/password.')
        return
      }
      // save token
      useGqlToken(result.login)
      navigateTo('/home')
    } catch(e) {
      console.log(e)
      alert('Invalid email/password.')
    }
  
  }
  onMounted(() => {
    if (localStorage.getItem('gql:default')) {
      navigateTo('/home')
    }
  })
</script>

<template>
  <div>
    <NuxtLayout name="auth">
      <v-sheet class="mx-auto pa-4 mt-12" width="600">
        <v-form ref="form">
          <v-text-field
            v-model="email"
            label="Email"
            required
          ></v-text-field>

          <v-text-field
            v-model="password"
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
              @click="handleLogin"
            >
              Login
            </v-btn>

            <v-btn
              class="mt-4"
              block
              @click="navigateTo('/register')"
            >
              Create Account
            </v-btn>
          </div>
         </v-form>
        </v-sheet>
    </NuxtLayout>
  </div>
</template>