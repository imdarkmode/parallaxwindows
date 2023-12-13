<template>
  <div>
    <video :srcObject="videoStream" :width="videoWidth" :height="videoHeight" autoplay
      :style="{ transform: 'translate(' + translateX + 'px,' + translateY + 'px)' }"></video>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, onBeforeMount } from 'vue';

let videoWidth = ref(0);
let videoHeight = ref(0);
let videoStream: any = ref(null);
let translateX = ref(0);
let translateY = ref(0);

onBeforeMount(() => {
  videoWidth.value = window.screen.availWidth;
  videoHeight.value = window.screen.availHeight;
  initCamera()
});

onMounted(() => {
  setInterval(setScreenDetails, 10);
});

onBeforeUnmount(() => {
  if (videoStream.value) {
    const tracks = videoStream.value.getTracks();
    tracks.forEach((track: any) => track.stop());
  }
});

const initCamera = async () => {
  try {
    await navigator.mediaDevices.getUserMedia({ video: true }).then((stream) => {
      videoStream.value = stream;
    })
  } catch (error) {
    console.error('Error accessing webcam:', error);
  }
};

function setScreenDetails() {
  translateX.value = -window.screenX
  translateY.value = -window.screenY
}

</script>

<style scoped lang="sass">
</style>