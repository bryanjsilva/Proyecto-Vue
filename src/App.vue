<template>
  <div class="d-flex flex-column">
    <main class="container mt-5">
      <h1 class="text-center text-light fw-bold">Mis gastos</h1>
      <Login 
      v-if='!login'
      v-bind:firebase='firebase'
      v-on:ingresoCorrecto='ingresoCorrecto'></Login>
    </main>
    <footer 
    v-if='!login'
    class="container-fluid mt-5 flex-start">
      <div class="row bg-light text-dark">
        <div class="col-6 d-flex justify-content-around">
          <span class="p-3">Contacto:</span>
          <a 
          href='https://www.facebook.com'
          target="_blank"
          class="p-3 text-primary"><i class="fa fa-facebook social"></i></a>
          <a 
          href='https://www.instagram.com'
          target="_blank"
          class="p-3 text-dark"><i class="fa fa-instagram social"></i></a>
          <a 
          href='https://www.linkedin.com/in/bryan-silva-9496261b2/'
          target="_blank"
          class="p-3 text-primary"><i class="fa fa-linkedin social"></i></a>
          <a 
          href='https://www.github.com/bryanjsilva'
          target="_blank"
          class="p-3 text-dark"><i class="fa fa-github social"></i></a>
        </div>
        <div class="col-6 text-center p-3">
          &copy; Todos los derechos reservados
        </div>
      </div> 
    </footer>
  </div>
</template>

<script>
import firebase from 'firebase/app'
import 'firebase/firestore'
import 'firebase/auth'

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
      userID: '',
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
      const settings = {timestampInSnapshots: true, merge:true};
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
      console.log(this.listaGastos)
  },
  methods:{
    ingresoCorrecto(dato){
      this.userID = dato
      this.login = true
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
  .social{
    transition: all 0.3s;
  }
  .social:hover{
    transform: scale(2);
    transition: all 0.3s;
  }
</style>