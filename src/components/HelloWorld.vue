<template>
  <div>
      
      <!-- <Navbar></Navbar>
      <Tab></Tab> -->
    <video ref="videoRef" :src="videoSrc" autoplay></video>
    <div>
      <v-btn variant="outlined">
Button
</v-btn>
      <v-btn variant="outlined" @click="startRecording" :disabled="isRecording">Start Recording</v-btn>
      <v-btn variant="outlined" @click="stopRecording" :disabled="!isRecording">Stop Recording</v-btn>
      <v-btn variant="outlined" @click="saveRecording" :disabled="!isRecording || !videoUrl">Save Recording</v-btn>
    </div>
  </div>
  <Tab></Tab>
</template>

<script>
// import Navbar from './Navbar.vue';
// import Tab from './Tab.vue';

export default {
  name : 'HelloWorld',
 
  data() {
      return {
          isRecording: false,
          mediaRecorder: null,
          recordedChunks: [],
          stream: null,
          videoUrl: "",
      };
  },
  computed: {
      videoSrc() {
          return this.stream ? this.$refs.videoRef.srcObject : null;
      },
  },
  methods: {
      startRecording() {
          navigator.mediaDevices.getUserMedia({ video: true })
              .then((stream) => {
              this.$refs.videoRef.srcObject = stream;
              this.stream = stream;
              this.recordedChunks = [];
              this.mediaRecorder = new MediaRecorder(stream);
              this.mediaRecorder.addEventListener("dataavailable", (event) => {
                  if (event.data.size > 0) {
                      console.log(event.data.size);
                      this.recordedChunks.push(event.data);
                  }
              });
              this.mediaRecorder.start();
              this.isRecording = true;
          })
              .catch((error) => {
              console.error("Error accessing webcam:", error);
          });
      },
      stopRecording() {
          if (this.mediaRecorder && this.mediaRecorder.state === "recording") {
              this.mediaRecorder.addEventListener("stop", () => {
                  const blob = new Blob(this.recordedChunks, { type: "video/webm" });
                  this.videoUrl = URL.createObjectURL(blob);
              });


              
              this.mediaRecorder.stop();
              this.isRecording = false;
              this.stream.getTracks().forEach((track) => track.stop());
          }
      },
      saveRecording() {
          if (this.videoUrl) {
              const a = document.createElement("a");
              a.href = this.videoUrl;
              a.download = "webcam_video.webm";
              document.body.appendChild(a);
              a.click();
              document.body.removeChild(a);
          }
      },
  },
};
</script>
