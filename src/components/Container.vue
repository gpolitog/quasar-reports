<template>
  <q-layout>
    <q-toolbar slot="header" color="dark">
      <q-toolbar-title>
        Rapporto giornaliero
        <span slot="subtitle"> {{ firstName }} {{ lastName }} </span>
        <span slot="subtitle"> {{ currentDate }} </span>
      </q-toolbar-title>
      <q-btn @click="logout" flat color="red">
        <q-icon name="person outline" />
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
import { QLayout, QTabs, QToolbarTitle, QRouteTab, QToolbar, QSelect, QIcon, QBtn, LocalStorage, date } from 'quasar'
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
        vehicleOptions: []
      },
      currentSite: LocalStorage.get.item('currentSite') || ''
    }
  },
  methods: {
    updateSite () {
      LocalStorage.set('currentSite', this.report.site)
    },
    logout () {
      LocalStorage.set('authToken', '')
      router.replace('/')
    }
  },
  mounted () {
    this.$http.post(this.config.routes.select, {token: LocalStorage.get.item('authToken')})
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

