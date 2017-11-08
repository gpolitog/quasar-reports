<template>
  <div>

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
  import { QDatetime, QSelect, QAutocomplete, QSlider, QInput, QField, QBtn } from 'quasar'

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
      return {
        meteoOptions: [{
          label: 'Sereno',
          value: 'sunny'
        }],
        report: {
          meteo: 'sunny',
          workStarted: null,
          workPaused: null,
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
            alert('ok')
          })
          .catch((error) => {
            alert(error)
          })
      }
    }
  }
</script>
