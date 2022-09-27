<template>
  <Modal :tipoEdicaoModal="tipoEdicaoModal" :campos="dadosModal" :labels="headers" :titulo="titulo" v-show="this.modalVisivel" @fechar=" (mr, d) => fecharModal(mr, d)"/>
  <table>
    <caption>{{ titulo }}</caption>
    <thead>
      <tr class="menu">
        <td :colspan="headers.length+2">
          <div class="pesquisa center inline" >
              <magnify-icon class="btn" title="Cancelar pesquisa"/>
              <input type="text" v-model="filtro" placeholder="Buscar">
              <botao :visivel="filtro !== ''" v-on:click="limpaFiltro"><template v-slot:icone><cancel-icon/></template></botao>
          </div>
          <div class="right inline">
            <botao :ativo="podeIncluir" backgroundColor="white" active-color="black" border="black 1px solid" v-on:click="abrirModal(-1, temInclusao)"><template v-slot:icone><plus-icon/><b>INCLUIR</b></template></botao>
          </div>
        </td>
      </tr>
      <tr>
        <th class="titulo" scope="col">#</th>
        <th class="titulo" scope="col" v-for="(h, index) in headers" :key="index">{{ h }}</th>
        <th class="titulo" scope="col">Opções</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(r, index) in dadosFiltrados" :key="index" class="center">
        <th scope="row">{{ index+1 }}</th>
        <td v-for="(d, i) in r" :key="i">{{ d }}</td>
        <td>
          <botao :ativo="podeVisualizar" activeColor="blue" v-on:click="abrirModal(index, temVisualizacao)"><template v-slot:icone><eye-icon/></template></botao>
          <botao :ativo="podeEditar" activeColor="green" v-on:click="abrirModal(index, temEdicao)"><template v-slot:icone><pencil-icon/></template></botao>
          <botao :ativo="podeExcluir" activeColor="red" v-on:click="abrirModal(index, temExclusao)"><template v-slot:icone><trash-can-icon/></template></botao>
        </td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <th :colspan="headers.length+2" class="rodape" v-if="filtro==''">
          Registros: {{ String(dados.length).padStart(2, "0") }}
        </th>
        <th :colspan="headers.length+2" class="rodape" v-else>
          Registros: {{ String(dadosFiltrados.length).padStart(2, "0") }}/{{ String(dados.length).padStart(2, "0")}}
        </th>
      </tr>
    </tfoot>
  </table>
</template>

<script>
/* eslint-disable */
import PlusIcon from 'vue-material-design-icons/Plus';
import PencilIcon from 'vue-material-design-icons/Pencil';
import TrashCanIcon from 'vue-material-design-icons/TrashCan';
import CancelIcon from 'vue-material-design-icons/Cancel';
import EyeIcon from 'vue-material-design-icons/Eye';
import MagnifyIcon from 'vue-material-design-icons/Magnify';
import Botao from "@/components/Botao";
import Modal from "@/components/Modal";
import {TipoEdicaoFormulario, ModalResult} from '@/consts'


export default {
  name: 'TabelaPadrao',
  components: {
    PlusIcon,
    PencilIcon,
    TrashCanIcon,
    CancelIcon,
    EyeIcon,
    MagnifyIcon,
    Botao,
    Modal,
  },
  props: {
    titulo: {
      default: 'TABELA',
      type: String,
      required: false
    },
    headers: {
      default: ['H1', 'H2', 'H3'],
      type: Array,
      required: false
    },
    dados: {
      default: [['D1', 'D2', 'D3'], ['D4', 'D5', 'D6']],
      type: Array,
      required: false
    },
    podeVisualizar: {
      default: true,
      type: Boolean,
      required: false,
    },
    podeIncluir: {
      default: true,
      type: Boolean,
      required: false,
    },
    podeEditar: {
      default: true,
      type: Boolean,
      required: false,
    },
    podeExcluir: {
      default: true,
      type: Boolean,
      required: false,
    },
  },
  data() {
    return {
      filtro: '',
      modalVisivel: false,
      temVisualizacao: TipoEdicaoFormulario.Visualizacao,
      temInclusao: TipoEdicaoFormulario.Inclusao,
      temEdicao: TipoEdicaoFormulario.Edicao,
      temExclusao: TipoEdicaoFormulario.Exclusao,
      tipoEdicaoModal: TipoEdicaoFormulario.Visualizacao,
      mrOk: ModalResult.Ok,
      mrCancel: ModalResult.Cancel,
      dadosModal: [],
      indiceModal: -1,
    }
  },
  methods: {
    limpaFiltro(){
      this.filtro = ''
    },
    fecharModal(modalResult, d){
      console.log(d)
      console.log(modalResult);
      this.modalVisivel = false;

      if(modalResult === this.mrOk){
        if(this.tipoEdicaoModal === this.temInclusao) {
          this.dados.push(d);
        } else if (this.tipoEdicaoModal === this.temEdicao) {
          this.dados[this.indiceModal] = d;
        } else if (this.tipoEdicaoModal === this.temExclusao){
          this.dados.splice(this.indiceModal, 1);
        }
      }
    },
    abrirModal(i, tem){
      if(tem === this.temInclusao && !this.podeIncluir ||
         tem === this.temVisualizacao && !this.podeVisualizar ||
         tem === this.temEdicao && !this.podeEditar ||
         tem === this.temExclusao && !this.podeExcluir){
        return
      }

      this.dadosModal = []
      if(tem !== this.temInclusao) {
        this.dadosModal = this.dados[i];
        this.indiceModal = i;
      }
      this.tipoEdicaoModal = tem;
      this.modalVisivel = true;
    },
  },
  computed: {
    dadosFiltrados() {
      if(this.filtro === ''){
        return this.dados;
      }
      return this.dados.filter(dado => {
        let valid = false
        dado.forEach(val => {
          if(val.toUpperCase().includes(this.filtro.toUpperCase())){
            valid = true
          }
        })
        return valid
      });
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

table {
  width: 100%;
  border-collapse: collapse;
}

caption {
  font-weight: bold;
  color: white;
  background: black;
  white-space: nowrap;
}

.titulo, .rodape {
  color: white;
  background: dimgray;
}

tr, td, th, caption {
  border: black 1px solid;
}

.center {
  text-align: center;
  justify-content: center;
  align-items: center;
}

.left {
  margin-left: 10px;
  float: left;
}

.right {
  margin-right: 10px;
  float: right;
}

.inline {
  display: inline-flex;
}

input {
  border: none;
  border-bottom: gray 1px solid;
  outline: none;
}

.menu {
  margin-bottom: 10px;
}

</style>
