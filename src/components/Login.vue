<template>
    <div class="row d-flex justify-content-center my-3">
        <div class="col-11 col-md-4 border rounded px-3 bg-light">
            <h2 class="text-center mt-3">Ingresar al sistema</h2>
            <form class="my-3">
                <div class="form-group">
                    <div class="form-floating mb-3">
                        <input type="email" class="form-control" id="floatingInput" placeholder="name@example.com">
                        <label for="floatingInput">Correo electrónico</label>
                    </div>
                    <div class="form-floating">
                        <input type="password" class="form-control" id="floatingPassword" placeholder="Password">
                        <label for="floatingPassword">Contraseña</label>
                    </div>
                </div>
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
                v-on:click.prevent='manejoClick($event)' 
                id='registro' 
                class="btn btn-outline-primary fw-bold w-100 mt-2 mb-3">
                    Regístrate &nbsp; <i class="fa fa-user"></i>
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Login',
    methods:{
        manejoClick(evento){
            if(evento.target.id==='ingreso'){
                this.$emit('ingresoCorrecto','ingreso')
            }
            if(evento.target.id==='registro'){
                this.$emit('ingresoCorrecto','registro')
            }
        }
    }
}
</script>
