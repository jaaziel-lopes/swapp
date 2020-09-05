<template>
  <div>
    <div id="header" class="text-center">
      <h1>Star Wars</h1>
      <h6 class="text-secondary">Listagem dos personagens do Universo Star Wars</h6>
    </div>
    <div id="main">
      <div class="container">
        <ListagemPessoas />
      </div>
    </div>
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.css";
import "font-awesome/css/font-awesome.css";
import ListagemPessoas from "./components/ListagemPessoas";

export default {
  name: 'app',
  components: {
    ListagemPessoas
  },
  data() {
    return {
    };
  }
}
</script>

<style>
#header {
  margin-top: 30px;
  margin-bottom: 30px;
}
#main {
  background-color: #F1F1F1;
  padding-top: 30px;
  padding-bottom: 30px;
}
</style>
