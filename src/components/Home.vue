<template>
  <div id="home">
    <div class="card-view">
      <h1>Frase de nuestro Brody el Mike</h1>
      <h3>
        <button @click="getPhrase" class="button btn-frase">Traer frase</button>
      </h3>
    </div>
    <div v-if="fraseMostrada" class="card-view">
      <h5>{{ frase }}</h5>
      <button class="button" v-clipboard="frase">Copiar</button>
    </div>
  </div>
</template>

<script>
  import toastr from 'toastr'

  let config = {
    apiKey: 'AIzaSyDli0vsxvOV3X9kkAazm9vOe2v8UBc6kcc',
    authDomain: 'frases-miguel.firebaseapp.com',
    databaseURL: 'https://frases-miguel.firebaseio.com',
    projectId: 'frases-miguel',
    storageBucket: 'frases-miguel.appspot.com',
    messagingSenderId: '288489979451'
  }

  let firebaseApp = firebase.initializeApp(config)
  let db = firebaseApp.database()

  export default {
    firebase: {
      frases: {
        source: db.ref('michael_phrases'),
        readyCallback: function () {
          this.frasesListas = true
          console.log('Listo!')
        }
      }
    },
    name: 'home',
    data () {
      return {
        frasesListas: false,
        fraseMostrada: false,
        frase: ''
      }
    },
    methods: {
      getPhrase () {
        if (!this.frasesListas) {
          toastr.error('Aún no están listas')
        } else {
          let index = parseInt(Math.random() * (this.frases.length - 1))
          this.frase = this.frases[index].phrase
          this.fraseMostrada = true
        }
      }
    },
    mounted () {}
  }
</script>

<style scoped>

  .card-view{
    margin-top: 30px;
    text-align: center;
    margin-left: 30px;
    margin-right: 30px;
    padding: 30px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
  }

  .btn-frase{
    margin-top: 15px;
  }

</style>
