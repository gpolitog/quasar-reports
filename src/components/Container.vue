<template>
  <q-layout>
    <q-toolbar slot="header" color="dark">
      <q-toolbar-title>
        Rapporto giornaliero
        <span slot="subtitle"> {{ firstName }} {{ lastName }} </span>
        <span slot="subtitle"> {{ currentDate }} </span>
      </q-toolbar-title>
      <q-btn @click="logoutDialog" flat color="red">
        <q-icon name="ion-android-exit" />
      </q-btn>
    </q-toolbar>
    <q-tabs slot="navigation" color="dark">
      <q-route-tab slot="title" icon="content paste" to="/container/report" replace label="Generale" />
      <q-route-tab slot="title" icon="directions car" to="/container/vehicle" replace label="Mezzi" />
      <q-route-tab slot="title" icon="photo camera" to="/container/picture" replace label="Foto" />
      <q-route-tab slot="title" icon="record voice over" to="/container/audio" replace label="Audio" />
    </q-tabs>

    <div id="main" class="column scroll overflow-hidden">
      <!-- SITE -->
      <q-select
      v-model="currentSite"
      v-on:change="updateSite"
      float-label="Sito"
      filter
      :options="ajaxOptions.siteOptions" />

      <router-view :ajax-options="ajaxOptions" />
    </div>
  </q-layout>
</template>

<script>
import { QLayout, QTabs, QToolbarTitle, QRouteTab, QToolbar, QSelect, QIcon,
  QBtn, LocalStorage, Dialog, date } from 'quasar'
import router from '../router'

const today = date.formatDate(Date.now(), 'DD-MM-YYYY')

export default {
  components: {
    QLayout,
    QTabs,
    QToolbarTitle,
    QRouteTab,
    QToolbar,
    QIcon,
    QBtn,
    QSelect
  },
  data () {
    return {
      firstName: LocalStorage.get.item('firstName'),
      lastName: LocalStorage.get.item('lastName'),
      currentDate: today,
      ajaxOptions: {
        siteOptions: [],
        vehicleOptions: [],
        userOptions: []
      },
      currentSite: ''
    }
  },
  methods: {
    updateSite () {
      this.$http.defaults.headers.common['site'] = this.currentSite
      LocalStorage.set('currentSite', this.currentSite)
    },
    logoutDialog () {
      const self = this
      Dialog.create({
        title: 'Logout',
        message: 'Sei autenticato come ' +
          LocalStorage.get.item('firstName') + ' ' +
          LocalStorage.get.item('lastName') + ', vuoi cambiare profilo?',
        buttons: [
          'Annulla',
          {
            label: 'Cambia profilo',
            handler () {
              self.logout()
            }
          }
        ]
      })
    },
    logout () {
      LocalStorage.set('authToken', '')
      LocalStorage.set('username', '')
      LocalStorage.set('firstName', '')
      LocalStorage.set('lastName', '')
      this.$http.defaults.headers.common['token'] = ''
      this.$http.defaults.headers.common['username'] = ''
      router.replace('/')
    }
  },
  mounted () {
    this.$http.post(this.config.routes.select)
      .then(result => {
        this.ajaxOptions.siteOptions = result.data.site.map(site => {
          return {
            label: site.name,
            value: site.code
          }
        })
        this.ajaxOptions.vehicleOptions = result.data.vehicle.map(vehicle => {
          return {
            label: vehicle.description,
            value: vehicle.plaque
          }
        })
        this.ajaxOptions.highwayOptions = result.data.highway.map(high => {
          return {
            label: high.name,
            value: high.name
          }
        })
        this.ajaxOptions.userOptions = result.data.user.map(user => {
          return {
            label: user.firstName + ' ' + user.lastName,
            value: user.firstName + ' ' + user.lastName
          }
        })
      })
      .catch(error => {
        if (error) {
          router.replace('/')
        }
      })
  }
}
</script>

<style lang="stylus">
  #main
    margin 15px
  .margin-bottom
    margin-bottom 10px
</style>

