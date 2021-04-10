<template>
  <v-app>
    <v-main id="main">
      <v-btn color="primary" @click="openCamera"><v-icon>mdi-plus</v-icon> Add Scan</v-btn>
      <v-bottom-sheet 
      v-model="camera"
      overlay-color="black"
      fullscreen 
      class="d-flex flex-column"
      >

        <div class="vc-container d-flex flex-column justify-end align-center">
          <video class="flex-grow-1" ref="video" autoplay></video>
          <div class="buttons">
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

            ><v-icon>mdi-camera</v-icon></v-btn>

                        <v-btn  
              @click="camera=!camera"
              fab
              elevation="0"
              small
            ><v-icon>mdi-close</v-icon></v-btn>

          </div>
        </div>

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
    imageCapture: null
  }),
  methods: {
    openCamera(){
      navigator.mediaDevices.getUserMedia({video: true}).then(mediaStream => {

        this.$refs.video.srcObject = mediaStream

        const track = mediaStream.getVideoTracks()[0];
        this.imageCapture = new ImageCapture(track);
      })
      .catch(error => console.log(error));   
      this.camera=true   
    },
    closeCamera(){

    }

  },
  mounted() {



  }
};
</script>


<style>
.v-main__wrap{
  display: flex;
  justify-content: center;
  align-items: center;
}

.vc-container {
  height: 100vh;
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

</style>