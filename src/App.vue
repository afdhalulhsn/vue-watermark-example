<template>
      <PageWatermark text="Afdhalul Ganteng" />
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <SensitiveInfo sensitiveInfo="Welcome to Your Vue.js App"/>

</template>

<script>
import SensitiveInfo from './components/SensitiveInfo.vue'
import PageWatermark from "@/components/PageWatermark.vue";


export default {
  name: 'App',
  components: {
    SensitiveInfo,
    PageWatermark
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
