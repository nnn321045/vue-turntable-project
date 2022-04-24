<script>
import Turntable  from "./components/Turntable.vue";

export default {
  components:{
    Turntable
  }
}
</script>

<template>
  <div id="app">
    <Turntable/>
  </div>
</template>

<style lang="scss">
*{
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100vw;
  height: 100vh;
  position: relative;
}
</style>