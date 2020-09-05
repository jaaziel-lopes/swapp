<template>
  <div>
    <br>
    <div class="col-md">
      <h4>Filmes</h4>
      <ul class="list-group">
        <li class="list-group-item nav-header">
          <div class="row">
            <div class="col-xs-8 col-sm-8 col-md-8">
              <b>Título</b>
            </div> 
            <div class="col-xs-4 col-sm-4 col-md-4">
              <b>Data</b>
            </div> 
          </div> 
        </li>
        <li class="list-group-item list-group-item-action" v-for="film in films" v-bind:key="film.id">
          <div class="row">
            <div class="col-xs-8 col-sm-8 col-md-8">
              {{film.title}}
            </div> 
            <div class="col-xs-4 col-sm-4 col-md-4">
              {{formatDate(film.releaseDate)}}
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

import "bootstrap/dist/css/bootstrap.css";
import "font-awesome/css/font-awesome.css";
import moment from 'moment';

export default {
  name: "ListagemFilmes",
  props: ["films"],
  methods: {
    formatDate(date) {
      return moment(String(date)).format("MM/DD/YYYY hh:mm");
    }
  }
};
</script>

<style scoped>
</style>