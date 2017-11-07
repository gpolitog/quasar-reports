<template>
  <q-layout>
    <q-toolbar slot="header">
      <q-toolbar-title>Rapporto giornaliero</q-toolbar-title>
    </q-toolbar>
    <q-tabs slot="navigation">
      <q-route-tab slot="title" icon="content paste" to="/container/report" replace label="Rapporto" />
      <q-route-tab slot="title" icon="photo camera" to="/container/picture" replace label="Foto" />
      <q-route-tab slot="title" icon="record voice over" to="/container/audio" replace label="Audio" />
    </q-tabs>

    <router-view />
  </q-layout>
</template>

<script>
import { QLayout, QTabs, QToolbarTitle, QRouteTab, QToolbar } from 'quasar'

export default {
  components: {
    QLayout,
    QTabs,
    QToolbarTitle,
    QRouteTab,
    QToolbar
  }
}
</script>

<style>
</style>

