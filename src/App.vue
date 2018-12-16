<template>
  <div id="app">
    <HelloWorld />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "app",
  components: {
    HelloWorld
  }
};
</script>

<style lang="sass">
body
  font-family: Roboto,sans-serif!important

h1
  margin-top: 50px
  text-align: center
</style>
