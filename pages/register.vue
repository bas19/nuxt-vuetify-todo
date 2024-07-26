<script setup>
  definePageMeta({
    layout: false,
  })
  import { reactive } from 'vue'
  import { useVuelidate } from '@vuelidate/core'
  import { required } from '@vuelidate/validators'

  const initialState = {
    name: '',
    email: '',
    password: '',
    confirm_password: '',
  }
  const state = reactive({
    ...initialState,
  })
  const rules = {
    name: { required },
    email: { required },
    password: { required },
  }
  
  const v$ = useVuelidate(rules, state)

  const register = async () => {
    v$.value.$validate()
    if (state.password !== state.confirm_password) {
      alert('Password not match.')
      return
    }
    if ( v$.value.$errors.length === 0) {
      try {
        const result = await GqlCreateUserMutation({ 
          name: state.name,
          email: state.email,
          password: state.password,
        })
        console.log(result)
        if (result.createUser) {
          alert('Registration successful.')
          navigateTo('/')
        }
      } catch(e) {
        alert("Server error, please try different email.")
        console.log(e)
      }
    }
  }
</script>

<template>
  <div>
    <NuxtLayout name="auth">
      <v-sheet class="mx-auto pa-4 mt-12" width="600">
        <v-form ref="form">

          <v-text-field
            v-model="state.name"
            label="Name"
            required
            :error-messages="v$.name.$errors.map(e => e.$message)"
            @blur="v$.name.$touch"
            @input="v$.name.$touch"
          ></v-text-field>

          <v-text-field
            v-model="state.email"
            label="Email"
            :error-messages="v$.name.$errors.map(e => e.$message)"
            @blur="v$.name.$touch"
            @input="v$.name.$touch"
          ></v-text-field>


          <v-text-field
            v-model="state.password"
            label="Password"
            clearable
            type="password"
            required
            :error-messages="v$.name.$errors.map(e => e.$message)"
            @blur="v$.name.$touch"
            @input="v$.name.$touch"
          ></v-text-field>

          <v-text-field
            v-model="state.confirm_password"
            label="Confirm Password"
            clearable
            type="password"
            required
            :error-messages="v$.name.$errors.map(e => e.$message)"
            @blur="v$.name.$touch"
            @input="v$.name.$touch"
          ></v-text-field>

          <div class="d-flex flex-column">
            <v-btn
              class="mt-4"
              color="secondary"
              block
              @click="register"
            >
              Register
            </v-btn>
            <v-btn
              class="mt-4"
              size="small"
              block
              @click="navigateTo('/')"
            >
              Back to Login
            </v-btn>
          </div>
         </v-form>
        </v-sheet>
    </NuxtLayout>
  </div>
</template>