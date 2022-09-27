<template>
  <div>
    <Tabela/>
    <br/>
    <Tabela :pode-editar="false"/>
    <br/>
    <Tabela :pode-incluir="false" :pode-excluir="false" :pode-visualizar="false"/>
    <br/>
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

* {
  margin: 0;
  padding: 0;
}

</style>
