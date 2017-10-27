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
    <q-knob
    class="self-center margin-bottom"
    v-model="report.travelTime"
    :min="0"
    :max="10"
    size="200px">
    {{report.travelTime}} ore di viaggio
    </q-knob>

    <!-- WORK TIME -->
    <q-knob
    class="self-center margin-bottom"
    v-model="report.workTime"
    :min="0"
    :max="10"
    size="200px">
    {{report.workTime}} ore di lavoro
    </q-knob>

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
    <q-select
    v-model="report.highwayEnter"
    float-label="Entrata autostrada"
    filter
    :options="highwayOptions" />

    <!-- HIGHWAY EXIT -->
    <q-select
    v-model="report.highwayExit"
    float-label="Uscita autostrada"
    filter
    :options="highwayOptions" />

    <!-- -->
  </div>
</template>

<script>
  import { QDatetime, QSelect, QKnob, QInput } from 'quasar'

  export default {
    components: {
      QDatetime,
      QSelect,
      QKnob,
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
      this.$http.get(this.config.routes.select)
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
            console.log(error)
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

