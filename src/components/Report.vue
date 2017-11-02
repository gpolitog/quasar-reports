<template>
  <div id="main" class="column scroll overflow-hidden">
    <!-- DATE -->
    <q-datetime
    v-model="report.date"
    type="date"
    monday-first
    float-label="Data" />

    <!-- METEO -->
    <q-select
    v-model="report.meteo"
    float-label="Meteo"
    :options="meteoOptions" />

    <!-- SITE -->
    <q-select
    v-model="report.site"
    float-label="Sito"
    filter
    :options="siteOptions" />

    <!-- WORK STARTED -->
    <q-datetime
    v-model="report.workStarted"
    type="time"
    float-label="Inizio giornata lavorativa" />

    <!-- WORK PAUSED -->
    <q-datetime
    v-model="report.workPaused"
    type="time"
    float-label="Pausa pranzo" />

    <!-- WORK STOPPED -->
    <q-datetime
    v-model="report.workStopped"
    type="time"
    float-label="Fine giornata lavorativa" />

    <!-- TRAVEL TIME -->
    <q-field
    :label="`${report.travelTime} ore di viaggio`">
      <q-slider
      class="self-center margin-bottom"
      v-model="report.travelTime"
      :min="0"
      :max="10" />
    </q-field>

    <!-- WORK TIME -->
    <q-field
    :label="`${report.workTime} ore di lavoro`">
      <q-slider
      class="self-center margin-bottom"
      v-model="report.workTime"
      :min="0"
      :max="10" />
    </q-field>

    <!-- VEHICLE -->
    <q-select
    v-model="report.vehicle"
    float-label="Automezzo di trasporto"
    filter
    :options="vehicleOptions" />

    <!-- VEHICLE KM -->
    <q-input
    v-model="report.vehicleKm"
    type="number"
    float-label="Kilometri percorsi" />

    <!-- HIGHWAY ENTER -->
    <q-input
    v-model="report.highwayEnter"
    float-label="Entrata autostrada">
      <q-autocomplete
      :static-data="highwayOptions" />
    </q-input>

    <!-- HIGHWAY EXIT -->
    <q-input
    v-model="report.highwayExit"
    float-label="Uscita autostrada">
      <q-autocomplete
      :static-data="highwayOptions" />
    </q-input>

    <!-- -->
  </div>
</template>

<script>
  import { QDatetime, QSelect, QAutocomplete, QSlider, QInput, QField, LocalStorage } from 'quasar'
  import router from '../router'

  export default {
    components: {
      QDatetime,
      QSelect,
      QAutocomplete,
      QSlider,
      QField,
      QInput
    },
    data () {
      return {
        siteOptions: [],
        vehicleOptions: [],
        highwayOptions: [],
        meteoOptions: [{
          label: 'Sereno',
          value: 'sunny'
        }],
        report: {
          date: new Date(),
          meteo: 'sunny',
          workStarted: null,
          workPaused: null,
          workStopped: null,
          travelTime: 0,
          workTime: 8,
          vehicleKm: 0
        }
      }
    },
    mounted () {
      this.$http.post(this.config.routes.select, {token: LocalStorage.get.item('authToken')})
        .then(result => {
          this.siteOptions = result.data.site.map(site => {
            return {
              label: site.name,
              value: site.code
            }
          })
          this.vehicleOptions = result.data.vehicle.map(vehicle => {
            return {
              label: vehicle.description,
              value: vehicle.plaque
            }
          })
        })
        .catch(error => {
          if (error) {
            router.push('/')
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

