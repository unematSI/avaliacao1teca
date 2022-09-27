<template>
  <img alt="logo" :src="logo"/>
  <div class="cabecalho">
    <h1> 1ª AVALIAÇÃO TECA</h1>
    <p>
      <b>Acadêmico:</b> André Luis Finger Candido
    </p>
    <p>
      <b>Professor:</b> Ivan
    </p>
  </div>
  <div>
    <h2>Tabela com cadastro de professores, aqui não é permitido edição dos registros</h2>
    <Tabela titulo="Professores" :headers="headerProfessores" :dados="dadosProfessors" :pode-editar="false"/>
    <br/>
    <h2>Tabela com cadastro de alunos, aqui não é permitido exclusão  dos registros</h2>
    <Tabela titulo="Alunos" :headers="headerAlunos" :dados="dadosAlunos" :pode-excluir="false"/>
    <br/>
    <h2>Tabela com cadastro de turmas, aqui é permitido apenas a edição dos registros</h2>
    <Tabela titulo="Turmas" :headers="headerTurmas" :dados="dadosTurmas" :pode-incluir="false" :pode-excluir="false" :pode-visualizar="false"/>
    <br/>
  </div>
</template>

<script>
import Tabela from "@/components/Tabela";
import logoUnemat from "@/assets/logo.png"

export default {
  name: 'App',
  components: {
    Tabela,
  },
  data () {
    return {
      logo: logoUnemat,
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
}
</script>

<style>

* {
  margin: 0;
  padding: 0;
}

img {
  margin: auto;
  display: block;
  height: 100px;
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
