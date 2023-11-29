<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <SpeakerBot />
</template>

<script>
import SpeakerBot from "./components/SpeakerBot.vue";

export default {
  name: "App",
  components: {
    SpeakerBot,
  },
};
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
}
</style>
