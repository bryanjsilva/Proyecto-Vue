/* En este archivo se integran los componentes ListaGastos y Login, además aquí se realiza la petición a firebase */

<template>
  <div class="d-flex flex-column">
    <main class="container mt-5">
      <div class='container d-flex'>
        <button 
        v-if='login'
        v-on:click='salirApp'
        class="btn btn-warning ms-auto">Salir</button>
      </div>
      <h1 class="text-center text-light fw-bold">Mis gastos</h1>
      <Login 
      v-if='!login'
      v-bind:firebase='firebase'
      v-on:ingresoCorrecto='ingresoCorrecto'
      v-on:registroCorrecto='registroCorrecto'></Login>
      <ListaGastos
      v-if='login'
      v-bind:listaGastos='listaGastos'
      v-on:eliminarGasto='eliminarGasto'
      v-on:editarGasto='editarGasto'
      v-on:agregarGasto='agregarGasto'></ListaGastos>
    </main>
    <footer 
    v-if='!login'
    class="container-fluid mt-5 flex-start">
      <div class="row bg-light text-dark">
        <div class="col-12 col-md-6 d-flex justify-content-around">
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
        <div class="col-12 col-md-6 text-center p-3">
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
import ListaGastos from './components/ListaGastos'

export default {
  name: 'App',
  components: {
    Login,
    ListaGastos
  },
  data: function(){
    return{
      login: false,
      listaUsuarios: [],
      gastos: {},
      listaGastos: [],
      firebase: '',
      userID: '',
      db: '',
      usuarios:{}
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

      // Se inicializa firebase con la configuración de la variable firebaseConfig

      firebase.initializeApp(firebaseConfig);
      this.db=firebase.firestore();
      const settings = {timestampInSnapshots: true, merge:true};
      this.db.settings(settings);
      this.firebase=firebase;
      // Se accede a la colleción usuarios y se obtienen los datos de los usuarios registrados en la app
      this.usuarios = this.db.collection('usuarios');
      this.usuarios.get().then(usuarios=>{
        usuarios.forEach(usuario=>{
          this.listaUsuarios.push({
            id: usuario.id,
            nombre: usuario.data().nombre,
            apellido: usuario.data().apellido,
            email: usuario.data().email
          })
        })
      })
      console.log(this.listaUsuarios)
  },
  methods:{
    // Si el ingreso desde el Login es correcto, se obtiene del usuario cada uno de los gastos que ha añadido en la app para mostrarlos en la pantalla
    ingresoCorrecto(dato){
      this.userID = dato
      this.login = true
      this.gastos=this.db.collection('usuarios').doc(this.userID).collection('libreta')
      this.gastos.get()
      .then(gastos=>{
        gastos.forEach(gasto=>{
          this.listaGastos.push({id: gasto.id, nombre:gasto.data().nombre, tipo: gasto.data().tipo, monto: gasto.data().monto})
        })
      })
      .catch(error=>{
        console.log('Error: '+error.code+' - '+error.message)
      })
      console.log(this.listaGastos)
    },
    // Si el usuario se ha registrado correctamente, se crea una colección libreta con un gasto de ejemplo para que el usuario pueda empezar a utilizar su libreta de gastos
    registroCorrecto(dato){
      this.usuarios.doc(dato.email).set(dato)
      this.usuarios.doc(dato.email).collection('libreta').add({
        nombre: 'ejemplo',
        monto: 9.99,
        tipo: 'Otro'
      })
    },
    // Cuando se llama a la función de eliminar gasto se elimina tanto de firestore como de la lista que maneja los gastos internamente
    eliminarGasto(dato){
      this.gastos.doc(dato.id).delete()
      this.listaGastos.forEach(element => {
          if(element.id===dato.id){
            this.listaGastos.splice(this.listaGastos.indexOf(element),1)
          }
        });
    },
    // Cuando se intenta agregar un gasto, no debe quedar el nombre en blanco, el nuevo gasto se añade tanto a firestore como a la lista que maneja los gastos internamente
    agregarGasto(dato){
      if(dato.nombre !== '' && dato.monto>0){
        this.gastos.add(dato)
        .then(doc=>{
          this.listaGastos.push({id: doc.id, nombre: dato.nombre,monto: dato.monto, tipo: dato.tipo})
          dato.nombre = ''
          dato.monto = ''
        })
        .catch(error=>{
          console.log('No se pudo agregar el gasto. Error: '+error.code+' - '+error.message)
        })
      }
    },
    // Si se quiere editar la información de un gasto se actualiza tanto en firestore como en la lista que maneja internamente los gastos
    editarGasto(gasto,id){
      this.gastos.doc(id.id).update(gasto)
      .then(()=>{
        this.listaGastos.forEach(element => {
          if(element.id===id.id){
            element.nombre = gasto.nombre
            element.tipo = gasto.tipo
            element.monto = gasto.monto
          }
        });
      })
      .catch(error=>{
        console.log('No se pudo actualizar el gasto. Error: '+error.code + ' - ' +error.message)
      })
    },
    // Si el usuario desea, puede cerrar sesión desde el botón asignado
    salirApp(){
      this.firebase.auth().signOut()
      .then(()=>{
        this.login = false
        this.listaGastos = []
      })
      .catch(error=>{
        console.log('No se ha cerrado la sesión correctamente. Error : '+error.code+' - '+error.message)
      })
    }
  }
}
</script>

<style>
  /* Se usa un gradiente para el fondo de la app */
  body{
    background: rgb(131,58,180);
    background: linear-gradient(90deg, rgba(131,58,180,1) 0%, rgba(252,99,69,1) 100%);
  }
  #app {
    font-family: 'Lato';
    color: #6338FC;
  }
  /* Se añade una transición para los íconos del footer */
  .social{
    transition: all 0.3s;
  }
  .social:hover{
    transform: scale(2);
    transition: all 0.3s;
  }
</style>