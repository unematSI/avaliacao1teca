<template>
  <div :class="btnClass" v-on:click="abreModal()" v-if="visivel">
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
    visivel: {
      default: true,
      type: Boolean,
      required: false
    },
    activeColor: {
      type: String,
      default: 'black',
      required: false
    },
    backgroundColor: {
      type: String,
      default: 'none',
      required: false
    },
    border: {
      type: String,
      default: 'black 0px solid',
      required: false
    }
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
  margin: 2px;
  padding: 2px;
  background-color: v-bind(backgroundColor);
  border: v-bind(border);
  border-radius: 5px;
}

.btn {
  cursor: pointer;
  color: v-bind(activeColor);
}

.disabledBtn {
  cursor: not-allowed;
  color: gray;
}

</style>
