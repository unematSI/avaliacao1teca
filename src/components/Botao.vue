<template>
  <div :class="btnClass" v-on:click="abreModal()">
    <slot name="icone"/>
  </div>
</template>

<script>

export default {
  name: 'botao-padrao',
  props: {
    ativo: {
      default: true,
      type: Boolean,
      required: false
    },
  },
  data() {
    return {
    }
  },
  computed: {
    btnClass(){
      return this.ativo ? 'btn' : 'disabledBtn'
    },
  },
  methods: {
    abreModal(){
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

div {
  text-align: center;
  display: inline-flex;
  justify-content: center;
  align-items: center;
}

.btn, .disabledBtn {
  padding: 4px;
}

.btn {
  cursor: pointer;
}

.disabledBtn {
  cursor: not-allowed;
  color: gray;
}

</style>
