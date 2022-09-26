<template>

  <table>
    <caption>{{ titulo }}</caption>
    <thead>
      <tr class="pesquisa">
          <text>Pesquisa:</text>
          <input type="text" v-on:change="filtraTabela" placeholder="Dado para busca..."/>
          <cancel-icon class="btn" title="Cancelar pesquisa"/>
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
<!--          <button> <plus-circle-icon/> </button>-->
          <pencil-icon class="btn"/>
          <trash-can-icon class="btn"/>

        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
/* eslint-disable */
import PlusIcon from 'vue-material-design-icons/Plus';
import PencilIcon from 'vue-material-design-icons/Pencil';
import TrashCanIcon from 'vue-material-design-icons/TrashCan';
import CancelIcon from 'vue-material-design-icons/Cancel';


export default {
  name: 'TabelaPadrao',
  components: {
    PlusIcon,
    PencilIcon,
    TrashCanIcon,
    CancelIcon,
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
    }
  },
  data() {
    return {
      filtro: '',
    }
  },
  methods: {
    filtraTabela(){
      return ''
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

.btn {
  padding: 4px;
  cursor: pointer;
}

</style>
