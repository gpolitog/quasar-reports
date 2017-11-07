<template>
  <div class="fullscreen column justify-center items-center">
    <q-input v-model="username" float-label="Nome utente" />
    <q-input v-model="password" type="password" float-label="Password" />

    <q-btn @click="login">Invia</q-btn>
  </div>
</template>

<script>
import { QInput, QBtn, LocalStorage } from 'quasar'

import router from '../router'

export default {
  components: {
    QInput,
    QBtn
  },
  data () {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    login () {
      const options = {
        username: this.username,
        password: this.password
      }
      this.$http.post(this.config.routes.login, options)
        .then(response => {
          LocalStorage.set('authToken', response.data.authToken)
          LocalStorage.set('username', response.data.username)
          router.replace('/container/report')
        })
        .catch((error) => {
          alert(error)
        })
    }
  }
}
</script>

<style>
</style>

