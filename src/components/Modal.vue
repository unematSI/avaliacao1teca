<template>
  <div class="bkg">
    <div class="modal">
      <header>
        {{ titulo }} - {{ tmeToStr() }}
      </header>
        <div v-for="(l, index) in labels" :key="index" class="campos">
          <p><b>{{ l }}:</b></p>
          <input :readonly="[TipoEdicaoFormulario.Visualizacao, TipoEdicaoFormulario.Exclusao].includes(tipoEdicaoModal)" v-model="dados[index]">
        </div>
      <footer class="rodape">
        <button type="button" class="btnCancelar botoes" @click="cancel">Cancelar</button>
        <button type="button" class="btnOk botoes" @click="confirm">OK</button>
      </footer>
    </div>
  </div>
</template>

<script>
import {TipoEdicaoFormulario, ModalResult} from '@/consts'
export default {
  name: "ModalPadrao",
  components: {

  },
  props: {
    titulo: {
      type: String,
      required: false,
      default: "Cadastro"
    },
    tipoEdicaoModal: {
      type: Number,
      required: true,
      default: TipoEdicaoFormulario.Visualizacao
    },
    labels: {
      type: Array,
      required: true
    },
    campos: {
      type: Array,
      required: true
    },
  },
  data (){
    return {
      TipoEdicaoFormulario,
      ModalResult,
      Result: null,
      dados: [],
    }
  },
  mounted() {
    this.dados = [];
    this.campos.forEach(dado => {
      this.dados.push(dado);
    })
  },
  methods: {
    isVisualizacao(){
      if(this.tipoEdicaoModal === TipoEdicaoFormulario.Visualizacao){
        this.Result = ModalResult.Cancel;
        this.$emit('fechar', this.Result, []);
        return true;
      }
      return false;
    },
    cancel(){
      if(this.isVisualizacao()){
        return;
      }
      if(confirm("Tem certeza que deseja cancelar o procecidimento?")) {
        this.Result = ModalResult.Cancel;
        this.$emit('fechar', this.Result, []);
      }
    },
    confirm(){
      if(this.isVisualizacao()){
        return;
      }
      this.Result = ModalResult.Ok;
      this.$emit('fechar', this.Result, this.dados);
    },
    tmeToStr(){
      return this.tipoEdicaoModal === TipoEdicaoFormulario.Edicao ? 'Alterar' :
          this.tipoEdicaoModal === TipoEdicaoFormulario.Visualizacao ? 'Visualizar' :
          this.tipoEdicaoModal === TipoEdicaoFormulario.Inclusao ? 'Incluir' : 'Excluir';
    }
  }
}
</script>

<style scoped>

.bkg {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: #FFFFFF;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
}


header {
  padding: 15px;
  display: flex;
  position: relative;
  border-bottom: 1px solid #eeeeee;
  color: black;
  font-weight: bold;
  justify-content: space-between;
}

.rodape{
  padding: 15px;
  border-top: 1px solid #eeeeee;
}

.campos {
  padding: 5px;
  margin: 5px;
}

.botoes {
  font-weight: bold;
  border-top: 1px solid #eeeeee;
  float: right;
  margin: 4px;
  padding: 4px;
}

.btnCancelar , .btnOk {
  color: white;
  border: 1px solid black;
  border-radius: 2px;
}

.btnCancelar {
  background: red;
}

.btnOk {
  background: blue;
}

input:read-only {
  background-color: rgba(0, 0, 0, 0.09);
  cursor: not-allowed;
}

</style>