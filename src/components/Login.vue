<template>
  <div class="fullscreen">
    <q-input v-model="username" float-label="Nome utente" />
    <q-input v-model="password" type="password" float-label="Password" />

    <q-btn @click="login">Invia</q-btn>
  </div>
</template>

<script>
import { QInput, QBtn } from 'quasar'

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
        .then(() => {
          alert('ok')
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

