<script setup>
import router from '@/router'
import { inject, reactive } from 'vue'
import { useAuthStore } from '@/stores/auth'

const auth = useAuthStore()

const data = reactive({
  username: '',
  password: '',
  snackbar: false,
  pesanLogin: ''
})

const myAxios = inject('myAxios')

const login = () => {
  console.log('login clicked', data)

  myAxios
    .post('/auth/login', {
      username: data.username,
      password: data.password
    })
    .then(
      (res) => {
        if (res.status == 200) {
          data.pesanLogin = 'Anda Berhasil Login'
          auth.authenticated()
          router.push('about')
        }
        data.snackbar = true
      },
      (err) => {
        data.pesanLogin = 'Username atau Password Salah'
        data.snackbar = true
      }
    )
}
</script>

<template>
  <v-card class="pa-2">
    <div class="container">
      <div>
        <label>Username</label>
        <v-text-field type="text" v-model="data.username" />
      </div>
      <div>
        <label>Password</label>
        <v-text-field type="password" v-model="data.password" />
      </div>
      <div>
        <!-- <button @click="login">Login</button> -->
        <v-btn @click="login">Login</v-btn>
      </div>
      <v-snackbar v-model="data.snackbar">
        {{ data.pesanLogin }}
        <template v-slot:actions>
          <v-btn color="pink" variant="text" @click="data.snackbar = false"> Close </v-btn>
        </template>
      </v-snackbar>
    </div>
  </v-card>
</template>
