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

    <!-- TODAY'S VEHICLES -->
    <q-list
     separator
     no-border
     style="margin: 20px;">
      <q-list-header>Veicoli aggiunti oggi</q-list-header>
      <q-item
      v-for="(vehicle, index) of vehicles"
      :key="index">
        <q-item-main>
          <q-item-tile label> {{vehicle.vehicle}} </q-item-tile>
          <q-item-tile sublabel> {{vehicle.vehicleKm}} Km </q-item-tile>
          <q-item-tile sublabel> Entrata in autostrada: {{vehicle.highwayEnter}} </q-item-tile>
          <q-item-tile sublabel> Uscita autostrada: {{vehicle.highwayExit}} </q-item-tile>
        </q-item-main>
        <q-item-side right>
          <q-btn
          round
          icon="delete"
          color="red"
          :vehicleId="vehicle._id"
          @click="deleteVehicle"></q-btn>
        </q-item-side>
      </q-item>
    </q-list>
  </div>
</template>

<script>
import { QInput, QAutocomplete, QSelect, QBtn,
  QList, QListHeader, QItem, QItemMain, QItemTile,
  QItemSide } from 'quasar'
export default {
  components: {
    QSelect,
    QAutocomplete,
    QBtn,
    QInput,
    QList,
    QListHeader,
    QItem,
    QItemMain,
    QItemTile,
    QItemSide
  },
  props: ['ajaxOptions'],
  data () {
    return {
      vehicles: [],
      report: {
        vehicle: '',
        vehicleKm: 0,
        highwayEnter: '',
        highwayExit: ''
      }
    }
  },
  mounted () {
    this.reloadVehicles()
  },
  methods: {
    send () {
      this.$http.post(this.config.routes.vehicle, this.report)
        .then(() => {
          this.reloadVehicles()
        })
        .catch(err => {
          if (err) alert('C\'è stato un errore e non è stato possibile inviare i dati')
        })
    },
    deleteVehicle (event) {
      const vehicleId = event.target.attributes.vehicleId.nodeValue
      this.$http.post(this.config.routes.deleteVehicle, {id: vehicleId})
        .then(() => {
          this.reloadVehicles()
        })
    },
    reloadVehicles () {
      this.$http.get(this.config.routes.getTodayVehicles)
        .then(response => {
          if (response.data) {
            this.vehicles = response.data
          }
        })
    }
  }
}
</script>
