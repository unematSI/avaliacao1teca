<template>

  <table>
    <caption>{{ titulo }}</caption>
    <thead>
      <tr class="pesquisa; center">
        <td :colspan="headers.length+2">
          <text>Pesquisa:</text>
          <input type="text" v-on:change="filtraTabela" placeholder="Dado para busca..."/>
          <cancel-icon class="btn" title="Cancelar pesquisa"/>
        </td>
      </tr>
      <tr>
        <th class="titulo" scope="col">#</th>
        <th class="titulo" scope="col" v-for="(h, index) in headers" :key="index">{{ h }}</th>
        <th class="titulo" scope="col">Opções</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(r, index) in dados" :key="index">
        <th scope="row">{{ index+1 }}</th>
        <td v-for="(d, i) in r" :key="i">{{ d }}</td>
        <td>
          <botao :ativo="podeEditar"><template v-slot:icone><pencil-icon/></template></botao>
          <botao :ativo="podeExcluir"><template v-slot:icone><trash-can-icon/></template></botao>
        </td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td :colspan="headers.length+2">
          <botao :ativo="podeIncluir"><template v-slot:icone><plus-icon/>Incluir</template></botao>
        </td>
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
import Botao from "@/components/Botao";
import TipoEdicaoModal from '@/const';


export default {
  name: 'TabelaPadrao',
  components: {
    PlusIcon,
    PencilIcon,
    TrashCanIcon,
    CancelIcon,
    Botao,
    TipoEdicaoModal,
  },
  props: {
    titulo: {
      default: 'TABELA',
      type: String,
      required: true
    },
    headers: {
      default: ['H1', 'H2', 'H3'],
      type: [],
      required: true
    },
    dados: {
      default: [['D1', 'D2', 'D3'], ['D4', 'D5', 'D6']],
      type: [],
      required: true
    },
    podeIncluir: {
      default: true,
      type: Boolean,
      required: true,
    },
    podeEditar: {
      default: true,
      type: Boolean,
      required: true,
    },
    podeExcluir: {
      default: true,
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      filtro: '',
    }
  },
  methods: {
    filtraTabela(){
      return ''
    },
    validaBotao(condicao){
      if(condicao){
        return 'btn'
      }
      return 'disabledBtn'
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

table {
  border-collapse: collapse;
  margin: auto;
}

caption {
  font-weight: bold;
  color: white;
  background: black;
  white-space: nowrap;
}

.titulo {
  color: white;
  background: dimgray;
}

tr, td, th, caption {
  padding: 4px;
  border: black 1px solid;
}

.center {
  text-align: center;
  display: table-;
}
</style>
