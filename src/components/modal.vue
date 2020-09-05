<template>
  <transition name="modal-fade">
    <div class="modal-backdrop">
      <div class="modal" role="dialog" aria-labelledby="modalTitle" aria-describedby="modalDescription">
        <header class="modal-header" id="modalTitle">
          <slot name="header">
            <h2>{{person.name}}</h2>
            <button type="button" class="btn-green" @click="close" aria-label="Voltar para a listagem">
              Voltar
            </button>
          </slot>
        </header>
        <section class="modal-body" id="modalDescription">
          <slot name="body">
            <div class="row" style="padding-left: 10px">
                <div class="col-md">
                    <strong>INFORMAÇÕES PESSOAIS</strong>
                </div>
            </div>
            <div  style="padding-left: 20px">
                <div class="row">
                    <div class="col-md">Nascido no ano <b>{{this.person.birthYear}}</b>.</div>
                </div>
                <div class="row">
                    <div class="col-md">&nbsp;&nbsp;Gênero <b>{{this.person.genderDescription}}</b>.</div>
                </div>
                <div class="row">
                    <div class="col-md">
                      <div style="display: inline-block;float: left">&nbsp;&nbsp;Cor dos ollhos:</div> 
                      <div :style="'display: inline-block;max-width:200px;width: 15px;height: 15px;margin-top: 6px;margin-left: 6px;border-radius: 50%;background-color:' + this.person.eyeColor" :title="this.person.eyeColor"></div>
                      |<div :style="'display: inline-block;max-width:220px;width: 15px;height: 15px;margin-top: 6px;margin-left: 5px;border-radius: 50%;background-color:' + this.person.eyeColor" :title="this.person.eyeColor"></div>
                    </div>
                </div>
                <ListagemFilmes v-bind:films="this.person.films" />
              </div>
          </slot>
        </section>
        <footer class="modal-footer">
          <hr>
        </footer>
      </div>
    </div>
  </transition>
</template>

<script>

import ListagemFilmes from "./ListagensFilmes";

export default {
    name: 'modal',
    props: ["person"],
    components: {
        ListagemFilmes
    },
    methods: {
        close() {
          this.$emit('close');
        },
    }
};
</script>

<style>
  .modal-backdrop {
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

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
    align-items: center;
  }

  .modal-header {
    border-bottom: 1px solid #eeeeee;
    color: #17a2b8;
    justify-content: space-between;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    justify-content: flex-end;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
  }

  .btn-green {
    color: white;
    background: #17a2b8;
    border: 1px solid #17a2b8;
    border-radius: 2px;
  }
</style>