<template>
  <div>
    <modal v-show="isModalVisible" v-bind:person="this.person" @close="closeModal" />
    <div class="row">
      <div class="col-md-8">
        Encontre quem está produrando:
        <input v-model="searchName" type="search" class="form-control" placeholder="Transcreva um fragmento da nomenclatura do indivíduo" />
      </div>
      <div class="col-md-4">
        Selecione o gênero:
        <select v-model="searchGender" class="custom-select">
          <option value="allGender" selected>Todos</option>
          <option value="male">Masculino</option>
          <option value="female">Feminino</option>
          <option value="hermaphrodite">Hermafrodita</option>
          <option value="n/a">Não definido</option>
          <option value="none">Não possui</option>
        </select>
      </div>
    </div>
    <br />
    <div class="col-md">
      <ul class="list-group">
        <li class="list-group-item nav-header">
          <div class="row">
            <div class="col-xs-6 col-sm-6 col-md-6">
              <b>Nome</b>
            </div> 
            <div class="col-xs-4 col-sm-4 col-md-4">
              <b>Gênero</b>
            </div> 
            <div class="col-xs-2 col-sm-2 col-md-2">
            </div> 
          </div> 
        </li>
        <li class="list-group-item list-group-item-action" v-for="person in filteredList" v-bind:key="person.name">
          <div class="row">
            <div class="col-xs-6 col-sm-6 col-md-6">
              {{person.name}}
            </div> 
            <div class="col-xs-4 col-sm-4 col-md-4">
              {{person.genderDescription}}
            </div> 
            <div class="col-xs-2 col-sm-2 col-md-2 text-right">      
              <button type="button" class="btn btn-info" aria-label="Mais detalhes" @click="showModal(person)">
                <span class="fa fa-eye"></span>
              </button>
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
import axios from "axios/dist/axios.js";
import modal from "./modal";

const genderMapConst = new Map([
  ["male", "Masculino"],
  ["female", "Feminino"], 
  ["hermaphrodite", "Hermafrodita"], 
  ["n/a", "Não definido"],
  ["none", "Não possui"]
]);

export default {
  name: "ListagemPessoas",
  components: {
    modal
  },
  data() {
    return {
      searchName: "",
      searchGender: "allGender",
      people: [],
      filmsMap: new Map(),
      person: {},
      isModalVisible: false
    };
  },
  mounted() {
    Promise.all([
      this.findFilms("https://swapi.dev/api/films/")
    ]).then(() => {
      Promise.all([
        this.findPeople("https://swapi.dev/api/people/")
      ]).then(() => {
        console.log(new Date);
      });
    });
  },
  methods: {
    getIdFilm(url) {
      return url.replace('http://swapi.dev/api/films/', '').replace('/', '');
    },
    findFilms(url) {
      console.log(new Date);
      return axios
      .get(url)
      .then(response => {
        const query = response.data;
        query.results.forEach(element => {
          let idFilm = this.getIdFilm(element.url);
          this.filmsMap.set(
            idFilm,
            {
              id: idFilm,
              title: element.title,
              releaseDate: element.release_date
            })
        });      
        if (query.next) {
          this.findFilms(query.next);
        }
      });
    },
    findPeople(url) {
      return axios
      .get(url)
      .then(response => {
        const query = response.data;
        this.people.push.apply(this.people, query.results.map(result => {
          return {
            name: result.name,
            birthYear: result.birth_year,
            gender: result.gender,
            genderDescription: this.getGenderDescription(result.gender),
            eyeColor: result.eye_color,
            films: result.films.map(film => this.filmsMap.get(this.getIdFilm(film)))
          };
        }));        
        if (query.next) {
          this.findPeople(query.next);
        }
      });
    },
    getGenderDescription(gender) {
      return genderMapConst.get(gender);
    },
    showModal(person) {
      this.person = person;
      this.isModalVisible = true;
    },
    closeModal() {
      this.person = [];
      this.isModalVisible = false;
    }
  },
  computed: {
    filteredList() {
      return this.people.filter(person => {
        return person.name.toLowerCase().includes(this.searchName.toLowerCase()) &&
         (this.searchGender == 'allGender' || person.gender.toLowerCase() == this.searchGender);
      })
    }
  }
};
</script>

<style scoped>
</style>
