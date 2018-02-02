<template>
  <div>
    <!-- TAKE A PICTURE -->
    <q-btn icon="photo camera"
    color="green"
    @click="takePicture">
      Scatta foto
    </q-btn>

    <!-- UPLOAD A PICTURE -->
    <q-uploader
    multiple
    :headers="headers"
    :url="url"
    style="margin-top: 10px;"/>
  </div>
</template>

<script>
  import { QBtn, QUploader, LocalStorage } from 'quasar'

  export default {
    components: {
      QBtn,
      QUploader
    },
    data () {
      return {
        picturePath: '',
        url: this.config.routes.pictures,
        headers: {
          token: LocalStorage.get.item('authToken'),
          username: LocalStorage.get.item('username'),
          site: LocalStorage.get.item('currentSite'),
          Connection: 'close'
        }
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
        options.headers = this.headers
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
