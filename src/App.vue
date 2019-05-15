<template>
  <v-app>
    <toast
      :key="0"
      position="se"
    />

    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Library</span>
        <span class="font-weight-light">Project</span>
      </v-toolbar-title>

      <v-spacer></v-spacer>
    </v-toolbar>

<!-- Componente pego no Vuetify -->
  <v-layout>
  <v-content>

    <v-text-field
      label="Título do livro" v-model="title"
     ></v-text-field>

    <v-text-field
      label="Número de Páginas" v-model="number"
    ></v-text-field>

    <v-btn @click="addBook()">Adicionar</v-btn>

    <v-flex v-for="(item, index) in allBooks" :key="index" xs12 sm6 offset-sm3>
      <v-card>
        <v-card-title primary-title>
          <div>
            <h3 class="headline mb-0"> {{ item.title }}</h3>
            <div> {{ item.pages }} páginas</div>
          </div>
        </v-card-title>
      </v-card>
    </v-flex>
  </v-content>
</v-layout>
  </v-app>
</template>

<script>
import axios from 'axios'
import Const from './helper/const'
import { Toast } from 'vuex-toast'

export default {
  components: {
    Toast
  },
  data () {
    return {
      allBooks: []
    }
  },
  /*A função mounted() é própria do Vue, ele executa quando a view é "montada"*/
  mounted () {
    console.log('mounted')
    this.getAllBooks()
  },
  methods: {
    getAllBooks () {
      return axios({
        method: 'get',
        url: Const.API_BOOKS
      })
        .then((response) => {
          console.log('Sucesso', response)
          this.allBooks = response.data
        })
        .catch((error) => {
          console.log('Erro', error)
        })
    },
    addBook () {
      const book = {
        title: this.title,
        pages: this.number
      }
      return axios({
        method: 'post',
        url: Const.API_BOOKS,
        data: book
      })
        .then((response) => {
          console.log('Sucesso', response)
          this.allBooks.push(book)
        })
        .catch((error) => {
          console.log('Erro', error)
        })
    }
  }
}
</script>
