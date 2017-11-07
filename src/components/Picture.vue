<template>
  <div id="main" class="column scroll overflow-hidden">

  <!-- PICTURES -->
    <q-btn icon="photo camera"
    color="primary"
    @click="takePicture">
      Invia foto
    </q-btn>

  </div>
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
          username: LocalStorage.get.item('username')
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

<style lang="stylus">
  #main
    margin 15px
  .margin-bottom
    margin-bottom 10px
</style>
