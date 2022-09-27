<template>
  <Modal :tipoEdicaoModal="tipoEdicaoModal" :campos="dadosModal" :labels="headers" :titulo="titulo" v-if="this.modalVisivel" @fechar=" (mr, d) => fecharModal(mr, d)"/>
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
            <botao :ativo="podeIncluir" backgroundColor="white" active-color="black" border="black 1px solid" v-on:click="abrirModal(-1, TipoEdicaoFormulario.Inclusao)"><template v-slot:icone><plus-icon/><b>INCLUIR</b></template></botao>
          </div>
        </td>
      </tr>
      <tr>
        <th class="titulo" scope="col">#</th>
        <th class="titulo" scope="col" v-for="(h, index) in headers" :key="index" v-on:click="ordenaDados(index)">
          <arrow-down-icon v-if="!sortingAsc && sortingIndex === index" :size="15"/>
          <arrow-up-icon v-if="sortingAsc && sortingIndex === index" :size="15"/>
          {{ h }}
        </th>
        <th class="titulo" scope="col">Opções</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(r, index) in dadosFiltrados" :key="index" :class="validaLinha(index)">
        <th scope="row">{{ index+1 }}</th>
        <td v-for="(d, i) in r" :key="i">{{ d }}</td>
        <td>
          <botao :ativo="podeVisualizar" activeColor="blue" v-on:click="abrirModal(index, TipoEdicaoFormulario.Visualizacao)"><template v-slot:icone><eye-icon/></template></botao>
          <botao :ativo="podeEditar" activeColor="green" v-on:click="abrirModal(index, TipoEdicaoFormulario.Edicao)"><template v-slot:icone><pencil-icon/></template></botao>
          <botao :ativo="podeExcluir" activeColor="red" v-on:click="abrirModal(index, TipoEdicaoFormulario.Exclusao)"><template v-slot:icone><trash-can-icon/></template></botao>
        </td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <th :colspan="headers.length+2" class="rodape" v-if="filtro===''">
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
import PlusIcon from 'vue-material-design-icons/Plus';
import PencilIcon from 'vue-material-design-icons/Pencil';
import TrashCanIcon from 'vue-material-design-icons/TrashCan';
import CancelIcon from 'vue-material-design-icons/Cancel';
import EyeIcon from 'vue-material-design-icons/Eye';
import MagnifyIcon from 'vue-material-design-icons/Magnify';
import ArrowUpIcon from 'vue-material-design-icons/ArrowUp';
import ArrowDownIcon from 'vue-material-design-icons/ArrowDown';
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
    ArrowUpIcon,
    ArrowDownIcon,
    Botao,
    Modal,
  },
  props: {
    titulo: {
      type: String,
      required: true
    },
    headers: {
      type: Array,
      required: true
    },
    dados: {
      type: Array,
      required: true
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
      TipoEdicaoFormulario,
      tipoEdicaoModal: TipoEdicaoFormulario.Visualizacao,
      mrOk: ModalResult.Ok,
      mrCancel: ModalResult.Cancel,
      dadosModal: [],
      indiceModal: -1,
      sortingAsc: true,
      sortingIndex: -1,
      fDados: [],
    }
  },
  mounted() {
    this.fDados = this.dados;
  },
  methods: {
    ordenaDados(i){
      if(this.sortingAsc) {
        this.dadosFiltrados.sort(function (a, b) {
          return (a[i] > b[i]) ? 1 : -1
        });
      } else{
        this.dadosFiltrados.sort(function (a, b) {
          return (a[i] > b[i]) ? -1 : 1
        });
      }
      this.sortingIndex = i;
      this.sortingAsc = !this.sortingAsc;
    },
    limpaFiltro(){
      this.filtro = ''
    },
    fecharModal(modalResult, d){
      this.modalVisivel = false;

      if(modalResult === this.mrOk){
        if(this.tipoEdicaoModal === TipoEdicaoFormulario.Inclusao) {
          this.fDados.push(d);
        } else if (this.tipoEdicaoModal === TipoEdicaoFormulario.Edicao) {
          this.fDados[this.indiceModal] = d;
        } else if (this.tipoEdicaoModal === TipoEdicaoFormulario.Exclusao){
          this.fDados.splice(this.indiceModal, 1);
        }
      }
    },
    abrirModal(i, tem){
      if(tem === TipoEdicaoFormulario.Inclusao && !this.podeIncluir ||
         tem === TipoEdicaoFormulario.Visualizacao && !this.podeVisualizar ||
         tem === TipoEdicaoFormulario.Edicao && !this.podeEditar ||
         tem === TipoEdicaoFormulario.Exclusao && !this.podeExcluir){
        return
      }

      this.dadosModal = []
      if(tem !== TipoEdicaoFormulario.Inclusao) {
        this.dadosModal = this.fDados[i];
        this.indiceModal = i;
      }
      this.tipoEdicaoModal = tem;
      this.modalVisivel = true;
    },
    validaLinha(i){
      if(i%2 === 0){
        return 'linha center par'
      }
      return 'linha center impar'
    }
  },
  computed: {
    dadosFiltrados() {
      if(this.filtro === ''){
        return this.fDados;
      }
      return this.fDados.filter(dado => {
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

<style scoped>

table {
  width: 100%;
  border-collapse: collapse;
  background-color: white;
}

caption {
  font-weight: bold;
  text-transform: uppercase;
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
  background-color: darkgray;
}

.par {
  background-color: rgba(0, 0, 0, 0.07);
}

.titulo{
  cursor: pointer;
}

.linha:hover{
  background-color: rgba(0, 0, 0, 0.2);
}

</style>
