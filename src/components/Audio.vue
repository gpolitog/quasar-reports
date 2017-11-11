<template>
  <q-btn icon="record voice over"
  color="green"
  @click="recordAudio">
    Invia audio
  </q-btn>
</template>

<script>
  import { QBtn, LocalStorage } from 'quasar'

  export default {
    components: {
      QBtn
    },
    data () {
      return {
        audioPath: ''
      }
    },
    methods: {
      recordAudio () {
        navigator.device.capture.captureAudio(mediaFiles => {
          this.audioPath = mediaFiles[0].fullPath
          this.send()
        }, () => {
          alert('Non è stato registrato o inviato nessun audio')
        })
      },
      send () {
        const fileURL = this.audioPath
        const ft = new window.FileTransfer()
        const options = new window.FileUploadOptions()
        options.fileKey = 'file'
        options.fileName = LocalStorage.get.item('username') + Date.now()
        options.mimeType = 'audio/mpeg'
        options.headers = {
          token: LocalStorage.get.item('authToken'),
          username: LocalStorage.get.item('username'),
          site: LocalStorage.get.item('currentSite'),
          Connection: 'close'
        }
        ft.upload(fileURL, this.config.routes.audio,
          () => {
            // File sent correctly
          },
          () => {
            // There were some errors
            alert('Ci sono stati degli errori e l\'audio non è stato inviato')
          }, options)
      }
    }
  }
</script>
