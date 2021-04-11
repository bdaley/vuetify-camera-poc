<template>
  <v-app>
    <v-main id="main">
      <v-btn color="primary" @click="openCamera"><v-icon>mdi-plus</v-icon> Add Scan</v-btn>
      <v-bottom-sheet 
      v-model="camera"
      overlay-color="black"
      fullscreen 
      class="d-flex flex-column"
      fixed
      >

        <div class="vc-container d-flex flex-column justify-start align-center">
          <video class="flex-grow-1" ref="video" autoplay></video>
          <img  class="d-none flex-grow-1" alt="" ref="image" />

        </div>

        <v-footer 
        class="buttons"
        fixed
        dark>
            <v-row
              justify="space-around"
              align="center"
              no-gutters
            >
              <v-btn  
                @click="camera=!camera"
                fab
                elevation="0"
                small
              ><v-icon>mdi-camera-switch-outline</v-icon></v-btn>

              <v-btn  
                fab
                color="primary"
                large
                elevation="0"
                @click="captureImage"

              ><v-icon>mdi-camera</v-icon></v-btn>

                          <v-btn  
                @click="closeCamera"
                fab
                elevation="0"
                small
              ><v-icon>mdi-close</v-icon></v-btn>

            </v-row>

          </v-footer>

        <!--  -->


        
      </v-bottom-sheet>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    camera: false,
    imageCapture: null,
    images: []
  }),
  methods: {
    async openCamera(){


      // TODO - Force fullscreen so we can lock the app in portrait mode
      try {
        await screen.orientation.lock('portrait')
      } catch (error) {
        console.log('Could not lock device orientation: ', error)
      }

      navigator.mediaDevices.getUserMedia({video: true}).then(mediaStream => {

        this.$refs.video.classList.remove('d-none');
        this.$refs.image.classList.add('d-none');


        this.$refs.video.srcObject = mediaStream

        const track = mediaStream.getVideoTracks()[0];
        this.imageCapture = new ImageCapture(track);
      })
      .catch(error => console.log(error));   
      this.camera=true   
    },
    closeCamera(){
      this.camera=false
    },
    captureImage(){
      this.imageCapture.takePhoto().then((blob) => {
          console.log('Took photo:', blob);
          this.$refs.image.classList.remove('d-none');
          this.$refs.video.classList.add('d-none');
          this.$refs.image.src = URL.createObjectURL(blob);
        }).catch(function(error) {
          console.log('takePhoto() error: ', error);
        });      
    }

  },
  mounted() {



  }
};
</script>


<style>
html, body, .v-application, .v-application--wrap {
  min-height: initial !important;
  height: 100% !important;
  
}
html { overflow-y: auto !important }
.v-main__wrap{
  display: flex;
  justify-content: center;
  align-items: center;
}

.vc-container {
  height: 100%;
}

.vc-container .buttons {
  width: 100%;
  background-color: black;
  padding: 20px;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.vc-container video {
  background-color: black;
}

.vc-container img {
  width:100%;
  height: 100%;
  object-fit: cover;
}

</style>