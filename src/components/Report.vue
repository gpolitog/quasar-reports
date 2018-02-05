<template>
  <div>

    <!-- SQUAD -->
    <q-select
    multiple
    toggle
    v-model="report.squad"
    float-label="Squadra"
    :options="ajaxOptions.userOptions" />

    <!-- METEO -->
    <q-select
    v-model="report.meteo"
    float-label="Meteo"
    :options="meteoOptions" />

    <!-- WORK STARTED -->
    <q-datetime
    v-model="report.workStarted"
    type="time"
    float-label="Inizio giornata lavorativa" />

    <!-- WORK PAUSE -->
    <q-select
    v-model="report.workPause"
    float-label="Durata pausa pranzo"
    :options="pauseOptions" />

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
      :max="maxTravelTime" />
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

    <!-- NOTE -->
    <q-input
    v-model="report.notes"
    type="textarea"
    float-label="Note"
    clearable />

    <!-- SEND -->
    <q-btn @click="send" color="green">
      Invia dati generali
    </q-btn>
  </div>
</template>

<script>
  import { QDatetime, QSelect, QAutocomplete, QSlider, QInput, QField, QBtn, LocalStorage } from 'quasar'
  const meteoOptions = require('../meteoOptions')
  const pauseOptions = require('../pauseOptions')

  export default {
    components: {
      QDatetime,
      QSelect,
      QAutocomplete,
      QSlider,
      QField,
      QBtn,
      QInput
    },
    props: ['ajaxOptions'],
    data () {
      const user = LocalStorage.get.item('firstName') + ' ' + LocalStorage.get.item('lastName')
      return {
        meteoOptions: meteoOptions,
        pauseOptions: pauseOptions,
        maxTravelTime: this.config.maxTravelTime,
        report: {
          squad: [user],
          meteo: '',
          workStarted: null,
          workPause: null,
          workStopped: null,
          travelTime: 0,
          workTime: 8,
          notes: ''
        }
      }
    },
    methods: {
      send () {
        this.$http.post(this.config.routes.report, this.report)
          .then(() => {
            alert('I dati generali sono stati inviati correttamente')
          })
          .catch(err => {
            if (err) alert('C\'è stato un errore e non è stato possibile inviare i dati')
          })
      }
    }
  }
</script>
