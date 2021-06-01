<template>
    <div class="row d-flex justify-content-center my-3">
        <div 
        v-if='!registro'
        class="col-11 col-md-4 border rounded px-3 bg-light">
            <h2 class="text-center mt-3">Ingresar al sistema</h2>
            <form class="my-3">
                <div class="form-group">
                    <div class="form-floating mb-3">
                        <input 
                        v-model='valorEmail'
                        type="email" 
                        v-bind:class='"form-control "+validEmail' 
                        id="floatingInput" placeholder="name@example.com">
                        <label for="floatingInput">Correo electrónico</label>
                    </div>
                    <div class="form-floating">
                        <input 
                        v-model='valorPass'
                        type="password" 
                        v-bind:class='"form-control "+validPass' id="floatingPassword" placeholder="Password">
                        <label for="floatingPassword">Contraseña</label>
                    </div>
                </div>
                <small 
                class="text-danger"
                v-if='mostrarMensaje'>
                    {{mensajeError}}
                </small>
                <button 
                v-on:click.prevent='manejoClick($event)'
                id='ingreso' 
                class="btn btn-primary mx-auto mt-3 w-100 fw-bold">
                    Ingresar &nbsp; <i class="fa fa-sign-in"></i>
                </button>
            </form>
            <hr>
            <div class="text-center">
                <label for="registro" class="form-label">¿Eres nuevo?</label>
                <button
                v-on:click.prevent='registro=true' 
                id='registro' 
                class="btn btn-outline-primary fw-bold w-100 mt-2 mb-3">
                    Regístrate &nbsp; <i class="fa fa-user"></i>
                </button>
            </div>
        </div>
        <div
        v-else
        class="col-11 col-md-8 border rounded px-3 bg-light">
            <h2 class="text-center mt-3">Registro de usuarios</h2>
            <form class='my-3 container'>
                <div class="row">
                    <div class="form-group col-6">
                        <label 
                        for="nombre"
                        class="form-label">Nombre</label>
                        <div class="input-group">
                            <span class="input-group-text">
                                <i class="fa fa-id-badge fa-fw"></i>
                            </span>
                            <input 
                            v-model='nombreRegistro'
                            type="text" 
                            name="nombre" 
                            id="nombre"
                            class="form-control">
                        </div> 
                    </div>
                    <div class="form-group col-6">
                        <label 
                        for="apellido"
                        class="form-label">Apellido</label>
                        <div class="input-group">
                            <span class="input-group-text">
                                <i class="fa fa-id-card fa-fw"></i>
                            </span>
                            <input 
                            v-model="apellidoRegistro"
                            type="text" 
                            name="apellido" 
                            id="apellido"
                            class="form-control">
                        </div>
                    </div>
                </div>
                <div class="row mt-2">
                    <div class="form-group col-12">
                        <label 
                        for="correo"
                        class="form-label">Correo electrónico</label>
                        <div class="input-group">
                            <span class="input-group-text">
                                <i class="fa fa-envelope fa-fw"></i>
                            </span>
                            <input 
                            v-model="emailRegistro"
                            type="email" 
                            name="correo" 
                            id="correo"
                            v-bind:class='"form-control "+validEmail'>
                        </div>
                    </div>
                </div>
                <div class="row mt-2">
                    <div class="form-group col-6">
                        <label 
                        for="contrasena"
                        class="form-label">Contraseña</label>
                        <div class="input-group">
                            <span class="input-group-text">
                                <i class="fa fa-lock fa-fw"></i>
                            </span>
                            <input 
                            v-model="passRegistro"
                            type="password" 
                            name="contrasena" 
                            id="contrasena"
                            v-bind:class='"form-control "+validPass'>
                        </div>
                    </div>
                    <div class="form-group col-6">
                        <label 
                        for="confirmar"
                        class="form-label">Confirmar contraseña</label>
                        <div class="input-group">
                            <span class="input-group-text">
                                <i class="fa fa-lock fa-fw"></i>
                            </span>
                            <input 
                            v-model="confirmarRegistro"
                            type="password" 
                            name="confirmar" 
                            id="confirmar"
                            v-bind:class='"form-control "+validConfirmar'>
                        </div>
                    </div>
                    <small 
                    v-if='mostrarError'
                    class="text-danger">{{errorRegistro}}</small>
                </div>
                <div class="row my-4 text-center">
                    <div class="col-6">
                        <button 
                        id="registrarse"
                        v-on:click.prevent="manejoClick($event)"
                        class="btn btn-primary">Registrarse </button>
                    </div>
                    <div class="col-6">
                        <button 
                        class="btn btn-outline-danger"
                        v-on:click.prevent='registro=false'>Cancelar </button>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Login',
    props: ['firebase'],
    data: function(){
        return{
            valorEmail: '',
            valorPass: '',
            mensajeError: '',
            mostrarMensaje: false,
            registro: false,
            emailRegistro: '',
            passRegistro: '',
            confirmarRegistro:'',
            nombreRegistro:'',
            apellidoRegistro:'',
            errorRegistro:'',
            mostrarError: false,
            validEmail: '',
            validPass: '',
            validConfirmar:''
        }
    },
    methods:{
        manejoClick(evento){
            if(evento.target.id==='ingreso'){
                if(this.valorEmail && this.valorPass !== ''){
                    this.mensajeError=''
                    this.validEmail=''
                    this.validPass=''
                    this.mostrarMensaje=false
                    this.firebase.auth().signInWithEmailAndPassword(this.valorEmail, this.valorPass)
                    .then((response) => {
                        console.log('Ingreso correcto')
                        this.$emit('ingresoCorrecto',response.user.email)
                    })
                    .catch((error) => {
                        console.log('Error: '+error.code+' - '+error.message);
                        if(error.code==='auth/user-not-found'){
                            this.mensajeError = 'El usuario no existe, debe registrarse'
                            this.validEmail='is-invalid'
                            this.mostrarMensaje=true
                        }else if(error.code==='auth/wrong-password'){
                            this.mensajeError = 'Contraseña incorrecta'
                            this.validPass='is-invalid'
                            this.mostrarMensaje=true
                        }
                    })
                }else{
                    this.mensajeError = 'Los campos no pueden estar vacíos'
                    this.mostrarMensaje = true
                }
            }
            if(evento.target.id==='registrarse'){
                this.errorRegistro=''
                this.validEmail=''
                this.validPass=''
                this.mostrarError=false
                if(this.passRegistro===this.confirmarRegistro){
                    this.firebase.auth().createUserWithEmailAndPassword(this.emailRegistro, this.passRegistro)
                    .then(response=>{
                        console.log('Registro correcto')
                        this.valorEmail = response.user.email
                        this.registro=false
                    })
                    .catch(error=>{
                        console.log('Error: '+error.code+' - '+error.message)
                        if(error.code==='auth/weak-password'){
                            this.errorRegistro='La contraseña debe tener al menos 6 caracteres'
                            this.mostrarError=true
                            this.validPass='is-invalid'
                        }
                        else if(error.code==='auth/invalid-email'){
                            this.errorRegistro='El correo electrónico ingresado no es válido'
                            this.mostrarError=true
                            this.validEmail='is-invalid'
                        }else if(error.code==='auth/email-already-in-use'){
                            this.errorRegistro='Este correo electrónico ya está en uso'
                            this.mostrarError=true
                            this.validEmail='is-invalid'
                        }
                    })
                }else{
                    this.errorRegistro='La contraseña no concide con la confirmación'
                    this.validConfirmar='is-invalid'
                    this.mostrarError=true
                }
            }
        }
    }
}
</script>
