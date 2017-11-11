<template>
  <div>
    <!-- VEHICLE -->
    <q-select
    v-model="report.vehicle"
    float-label="Automezzo di trasporto"
    filter
    :options="ajaxOptions.vehicleOptions" />

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
      :static-data="{field: 'value', list:ajaxOptions.highwayOptions}" />
    </q-input>

    <!-- HIGHWAY EXIT -->
    <q-input
    v-model="report.highwayExit"
    float-label="Uscita autostrada">
      <q-autocomplete
      :static-data="{field: 'value', list:ajaxOptions.highwayOptions}" />
    </q-input>

    <!-- SEND -->
    <q-btn @click="send" color="green">
      Invia dati relativi al mezzo {{ report.vehicle }}
    </q-btn>
  </div>
</template>

<script>
import { QInput, QAutocomplete, QSelect, QBtn } from 'quasar'
export default {
  components: {
    QSelect,
    QAutocomplete,
    QBtn,
    QInput
  },
  props: ['ajaxOptions'],
  data () {
    return {
      report: {
        vehicle: '',
        vehicleKm: 0,
        highwayEnter: '',
        highwayExit: ''
      }
    }
  },
  methods: {
    send () {
      this.$http.post(this.config.routes.vehicle, this.report)
        .then(() => {
          alert('I dati sul veicolo sono stati inviati correttamente')
        })
        .catch(err => {
          if (err) alert('C\'è stato un errore e non è stato possibile inviare i dati')
        })
    }
  }
}
</script>
