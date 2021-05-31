<template>
  <main class="container mt-5">
    <h1 class="text-center text-light fw-bold">Mis gastos</h1>
    <Login 
    v-if='!login'
    v-on:ingresoCorrecto='ingresoCorrecto'></Login>
  </main>
</template>

<script>
import firebase from 'firebase'

import Login from './components/Login'
export default {
  name: 'App',
  components: {
    Login
  },
  data: function(){
    return{
      login: false,
      listaGastos: [],
      gastos: {},
      firebase: '',
      db: ''
    }
  },
  beforeMount(){
      const firebaseConfig = {
        apiKey: "AIzaSyCdIUSjkIUd49JoGW3YHZqNvbFkI7tPEc0",
        authDomain: "proyecto-vue-bs-37e6f.firebaseapp.com",
        projectId: "proyecto-vue-bs-37e6f",
        storageBucket: "proyecto-vue-bs-37e6f.appspot.com",
        messagingSenderId: "825583609859",
        appId: "1:825583609859:web:dfadcb001efc73faff8636",
        measurementId: "G-VZJWRW3RWT"
      };

      firebase.initializeApp(firebaseConfig);
      this.db=firebase.firestore();
      const settings = {timestampInSnapshots: true};
      this.db.settings(settings);
      this.firebase=firebase;

      this.gastos = this.db.collection('usuarios');
      this.gastos.get().then(gastos=>{
        gastos.forEach(gasto=>{
          this.listaGastos.push({
            id: gasto.id,
            nombre: gasto.data().nombre,
            tipo: gasto.data().tipo,
            monto: gasto.data().monto
          })
        })
      })
  },
  methods:{
    ingresoCorrecto(dato){
      alert(dato)
    }
  }
}
</script>

<style>
  body{
    background: rgb(131,58,180);
    background: linear-gradient(90deg, rgba(131,58,180,1) 0%, rgba(252,99,69,1) 100%);
  }
  #app {
    font-family: 'Lato';
    color: #6338FC;
  }
</style>