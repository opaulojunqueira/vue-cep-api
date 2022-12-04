<template>
  <center>
    <div class="container" id="app">
      <a class="navbar-brand text-white items" href="#">
        <img src="../public/favicon.png" alt="Logo" width="30" height="30" class="d-inline-block align-text-top items">
        <h4>Pesquisar CEP</h4>
      </a>
      <br>
      <input type="text" id="cep" name="cep" v-model="cep" placeholder="Digite o cep aqui" class="items">
      <button @click="getCep()" data-bs-toggle="modal" data-bs-target="#staticBackdrop" class="items pesquisar">🔎
        Pesquisar</button>
    </div>
  </center>
  <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
    aria-labelledby="staticBackdropLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="staticBackdropLabel">🔎 Resultado da pesquisa...</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <h6>{{ message }}</h6>
          <h6>{{ localidade }}</h6>
          <h6>{{ ddd }}</h6>
          <h6>{{ bairro }}</h6>
          <h6>{{ logradouro }}</h6>
          <h6>{{ complemento }}</h6>
          <h6>{{ informacoes }}</h6>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">✖ Fechar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "CEP",
  data() {
    return {
      cep: null,
      bairro: null,
      complemento: null,
      localidade: null,
      logradouro: null,
      informacoes: null,
      message: null
    }
  },
  methods: {
    async getCep() {
      if (this.cep == null || this.cep == '') {
        this.message = '🤔 O campo de pesquisa está vázio.'
        this.localidade = null
        this.bairro = null
        this.logradouro = null
        this.complemento = null
        this.informacoes = null
      } else {
        if (this.cep.length == 8) {
          console.log('--> Pesquisa sendo realizada!')
          axios.get(`https://viacep.com.br/ws/${this.cep}/json/`).then((result) => {

            if (result.data.cep == undefined) {
              this.message = `🤔 O CEP inserido é inválido, por favor verifique-o e tente novamente.`
              this.localidade = null
              this.bairro = null
              this.logradouro = null
              this.complemento = null
              this.informacoes = null
            } else {
              this.message = `📍 CEP: ${result.data.cep}`
              this.localidade = `📍 Cidade: ${result.data.localidade != '' ? result.data.localidade : 'Não definido'} - ${result.data.uf != '' ? result.data.uf : 'Não definido'}`
              this.bairro = `📍 Bairro: ${result.data.bairro != '' ? result.data.bairro : '❌'}`
              this.logradouro = `📍 Logradouro: ${result.data.logradouro != '' ? result.data.logradouro : '❌'}`
              this.complemento = `📍 Complemento: ${result.data.complemento != '' ? result.data.complemento : '❌'}`
              this.informacoes = `📍 DDD: ${result.data.ddd != '' ? result.data.ddd : '❌'} | IBGE: ${result.data.ibge != '' ? result.data.ibge : '❌'} | GIA: ${result.data.gia != '' ? result.data.gia : '❌'} | SIAFI: ${result.data.siafi != '' ? result.data.siafi : '❌'}`
            }

          }).catch((error) => {
            console.log(error)
          })

        } else {
          this.message = '🤔 O campo de pesquisa não tem os números necessários.'
          this.localidade = null
          this.bairro = null
          this.logradouro = null
          this.complemento = null
          this.informacoes = null
        }
      }
    }
  }
}
</script>

<style scoped>
.container {
  width: 500px;
  height: 200px;
  border-radius: 15px;
  margin-top: 100px;
  background-color: #212529;
  font-family: Arial, Helvetica, sans-serif;
}

.items {
  margin-top: 20px;
}

input {
  padding: 5px 5px 5px;
  background-color: #FFF;
  border: solid #212529;
  text-align: center;
  width: 180px;
}

button.pesquisar {
  margin-left: 15px;
  padding: 5px 5px 5px;
  border: solid #212529;
  background-color: #FFF;
  color: #212529;
}

button:hover.pesquisar {
  border: solid #FFF;
  background-color: #212529;
  color: #FFF;
}
</style>
