<template>

  <!-- PICTURES -->
  <q-btn icon="photo camera"
  color="green"
  @click="takePicture">
    Invia foto
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
        picturePath: ''
      }
    },
    methods: {
      takePicture () {
        navigator.device.capture.captureImage(mediaFiles => {
          this.picturePath = mediaFiles[0].fullPath
          this.send()
        }, () => {
          alert('Non è stata salvata o inviata nessuna immagine')
        })
      },
      send () {
        const ft = new window.FileTransfer()
        const options = new window.FileUploadOptions()
        options.fileKey = 'file'
        options.mimeType = 'image/jpeg'
        const fileURL = this.picturePath
        options.fileName = LocalStorage.get.item('username') + Date.now()
        options.headers = {
          token: LocalStorage.get.item('authToken'),
          username: LocalStorage.get.item('username'),
          site: LocalStorage.get.item('currentSite')
        }
        ft.upload(fileURL, this.config.routes.picture,
          () => {
            // File sent correctly
          },
          () => {
            // There were some errors
            alert('Ci sono stati degli errori e l\' immagine non è stata inviata.')
          }, options)
      }
    }
  }
</script>
