<template>
  <img alt="logo" :src="logo"/>
  <div class="cabecalho">
    <h1> 1ª AVALIAÇÃO TECA</h1>
    <p>
      <b>Acadêmico:</b> André Luis Finger Candido
    </p>
    <p>
      <b>Professor:</b> Ivan Luiz Pedroso Pires
    </p>
  </div>
  <div>
    <nav class="menu">
      <ul>
        <li>
          <a href="#" v-on:click="alternaTabelas('Professores')">
            PROFESSORES
          </a>
        </li>
        <li>
          <a href="#" v-on:click="alternaTabelas('Alunos')">
            ALUNOS
          </a>
        </li>
        <li>
          <a href="#" v-on:click="alternaTabelas('Turmas')">
            TURMAS
          </a>
        </li>
      </ul>
    </nav>
    <div v-if="exibeProfessores">
    <h2>Tabela com cadastro de professores, aqui não é permitido edição dos registros</h2>
    <Tabela titulo="Professores" :headers="headerProfessores" :dados="dadosProfessors" :pode-editar="false"/>
    </div>
    <div v-if="exibeAlunos">
    <h2>Tabela com cadastro de alunos, aqui não é permitido exclusão  dos registros</h2>
    <Tabela titulo="Alunos" :headers="headerAlunos" :dados="dadosAlunos" :pode-excluir="false"/>
    </div>
    <div v-if="exibeTurmas">
    <h2>Tabela com cadastro de turmas, aqui é permitido apenas a edição dos registros</h2>
    <Tabela titulo="Turmas" :headers="headerTurmas" :dados="dadosTurmas" :pode-incluir="false" :pode-excluir="false" :pode-visualizar="false"/>
    </div>
  </div>
</template>

<script>
import Tabela from "@/components/Tabela";
import logoUnemat from "@/assets/logo.png"
import bkg from "@/assets/bkg.jpg"

export default {
  name: 'App',
  components: {
    Tabela,
  },
  data () {
    return {
      exibeProfessores: true,
      exibeAlunos: false,
      exibeTurmas: false,
      logo: logoUnemat,
      bkg: bkg,
      headerProfessores: ['Nome', 'Email'],
      dadosProfessors: [
        ['Ivan', 'ivan@unemat.br'],
        ['Tales', 'tales@unemat.br'],
      ],
      headerAlunos: ['Nome', 'Email', 'Data de nascimento'],
      dadosAlunos: [
        ['André', 'andre.luis@unemat.br', '19/06/1996'],
        ['Allan', 'allan@unemat.br', '18/05/1991'],
      ],
      headerTurmas: ['Disciplina', 'Horário', 'Professor', 'Campus'],
      dadosTurmas: [
        ['ESTRUTURA DE DADOS II', '2N1234', 'Ramilton', 'Aquarela'],
        ['SISTEMAS OPERACIONAIS', '3N123', 'Maicon', 'Aquarela'],
        ['TECA', '4N123', 'Ivan', 'Imperial'],
        ['LPOO', '5N123', 'Zhan', 'Aquarela'],
        ['SEGURANÇA COMPUTACIONAL', '6N123', 'Ramilton', 'Aquarela'],
        ['TCC I', '3N4 (08/08/2022 - 22/11/2022), 2356N1234 (28/11/2022-15/12/2022)', 'Morgan', 'Aquarela'],
      ],
    }
  },
  methods: {
    alternaTabelas(btn){
      this.exibeProfessores = btn === 'Professores';
      this.exibeAlunos = btn === 'Alunos';
      this.exibeTurmas = btn === 'Turmas';
    }
  }
}
</script>

<style>
html {
  background-image: url('@/assets/bkg.jpg');
}

* {
  margin: 0;
  padding: 0;
}

.menu {
  background-color: rgba(0, 0, 0, 0.2);
}

.menu ul {
  max-width: 800px;
  list-style: none;
  padding: 0;
}

.menu ul li {
  display: inline;
}

.menu ul li a {
  color: #FFF;
  padding: 20px;
  display: inline-flex;
  text-decoration: none;
  transition: background .4s;
}

.menu ul li a:hover {
  color: black;
}


img {
  margin: auto;
  display: block;
  height: 100px;
}

.cabecalho, h2 {
  color: white;
}

.cabecalho {
  padding: 20px;
  text-align: center;
}

div {
  padding-top: 10px;
}

h2 {
  padding-left: 10px;
}

</style>
