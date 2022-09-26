<template>
  <div>
  <Tabela/>
  </div>
</template>

<script>
// import HelloWorld from './components/Tabela.vue'
import Tabela from "@/components/Tabela";

export default {
  name: 'App',
  components: {
    Tabela
    // HelloWorld
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
