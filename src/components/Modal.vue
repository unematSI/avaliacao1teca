<template>
  <div class="bkg">
    <div class="modal">
      <header>
        {{ titulo }} - {{ tmeToStr() }}
      </header>
        <div v-for="(l, index) in labels" :key="index" class="campos">
          <p><b>{{ l }}:</b></p>
          <input :readonly="[temVisualizacao, temExclusao].includes(tipoEdicaoModal)" v-model="dados[index]">
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
      default: "Titulo"
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
      temVisualizacao: TipoEdicaoFormulario.Visualizacao,
      temInclusao: TipoEdicaoFormulario.Inclusao,
      temEdicao: TipoEdicaoFormulario.Edicao,
      temExclusao: TipoEdicaoFormulario.Exclusao,
      mrOk: ModalResult.Ok,
      mrCancel: ModalResult.Cancel,
      Result: null,
      dados: [],
    }
  },
  watch: {
    campos(newCampos) {
      this.dados = [];
      newCampos.forEach(dado => {
        this.dados.push(dado);
      })
    },
  },
  methods: {
    cancel(){
      if(this.tipoEdicaoModal === this.temVisualizacao){
        this.Result = this.mrCancel;
        this.$emit('fechar', this.Result, []);
        return;
      }
      if(confirm("Tem certeza que deseja cancelar o procecidimento?")) {
        this.Result = this.mrCancel;
        this.$emit('fechar', this.Result, []);
      }
    },
    confirm(){
      if(this.tipoEdicaoModal === this.temVisualizacao){
        this.Result = this.mrCancel;
        this.$emit('fechar', this.Result, []);
        return;
      }
      this.Result = this.mrOk;
      this.$emit('fechar', this.Result, this.dados);
    },
    tmeToStr(){
      return this.tipoEdicaoModal === this.temEdicao ? 'Alterar' :
          this.tipoEdicaoModal === this.temVisualizacao ? 'Visualizar' :
          this.tipoEdicaoModal === this.temInclusao ? 'Incluir' : 'Excluir';
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

</style>