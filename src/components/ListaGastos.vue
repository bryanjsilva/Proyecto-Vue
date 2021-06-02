<template>
    <section class="row">
        <div class="col-11 border rounded bg-light mx-auto my-3">
            <div class="d-flex justify-content-around m-3">
                <h4 class="align-self-end">Agregar gasto</h4>
                <button 
                v-bind:id='iconoAgregar'
                v-on:click.prevent="manejoClick($event)"
                class="btn btn-warning">
                    <i v-show='!mostrarAgregar' id='iconoMas' class="fa fa-plus" v-on:click.prevent="manejoClick($event)"></i>
                    <i v-show='mostrarAgregar' id='iconoMenos' class='fa fa-minus' v-on:click.prevent="manejoClick($event)"></i>
                </button>
            </div>
            <form id='agregarGasto' class="container mt-3 mb-2 card contenido none">
                <div class="row justify-content-center m-2">
                    <div class="col-12 col-md-5">
                        <label class="form-label" for='nombreGasto'>Nombre del gasto</label>
                    </div>
                    <div class="col-12 col-md-5">
                        <input type="text" name="nombreGasto" id="nombreGasto" class="form-control">
                    </div>
                </div>
                <div class="row justify-content-center m-2">
                    <div class="col-12 col-md-5">
                        <label class="form-label" for='montoGasto'>Monto del gasto</label>
                    </div>
                    <div class="col-12 col-md-5">
                        <input type="number" name="montoGasto" id="montoGasto" class="form-control" min='0'>
                    </div>
                </div>
                <div class="row justify-content-center m-2">
                    <div class="col-12 col-md-5">
                        <label class="form-label" for='tipoGasto'>Tipo del gasto</label>
                    </div>
                    <div class="col-12 col-md-5">
                        <select name="tipoGasto" id="tipoGasto" class="form-select">
                            <option  selected value="Entretenimiento">Entretenimiento</option>
                            <option value="Hogar">Hogar</option>
                            <option value="Salud">Salud</option>
                            <option value="Educación">Educación</option>
                            <option value="Otro">Otro</option>
                        </select>
                    </div>
                </div>
                <div class="row justify-content-center m-2">
                    <div class="col-12 col-md-4">
                        <button 
                        v-on:click.prevent='agregarGasto'
                        class="btn btn-primary w-100">
                            Agregar
                        </button>
                    </div> 
                </div>
            </form>
            <section class="container card text-center my-3">
                <div class="row h5 mt-3">
                    <div class="col-4">
                        Nombre
                    </div>
                    <div class="col-3">
                        Monto
                    </div>
                    <div class="col-3">
                        Tipo
                    </div>
                    <div class="col-2">
                        Acción
                    </div>
                </div>
                <hr>
                <div 
                v-for='(gasto,index) in listaGastos'
                v-bind:key='index'
                class="row my-3 text-center">
                    <div class="col-4">
                        {{gasto.nombre}}
                    </div>
                    <div class="col-3">
                        {{gasto.monto}}
                    </div>
                    <div class="col-3">
                        {{gasto.tipo}}
                    </div>
                    <div class="col-2 justify-content-around">
                        <i 
                        v-on:click="manejoGasto($event,gasto.id)"
                        id='editar'
                        class="fa fa-edit fa-fw accion text-warning m-2"></i>
                        <i 
                        v-on:click="manejoGasto($event,gasto.id)"
                        id='eliminar'
                        class="fa fa-trash fa-fw accion text-danger m-2"></i>
                    </div>
                </div>
            </section>
        </div>
    </section>
</template>

<script>
export default {
    name: 'ListaGastos',
    props: ['listaGastos'],
    data: function(){
        return{
            mostrarAgregar: false,
            iconoAgregar: 'iconoAgregar'
        }
    },
    methods:{
        manejoClick(evento){
            if(evento.target.id==='iconoAgregar' || evento.target.id==='iconoMas'){
                this.iconoAgregar='iconoEliminar'
                this.mostrarAgregar = true
                const contenido = document.getElementById('agregarGasto')
                contenido.classList.add('block')
                contenido.classList.remove('none')
            }else if(evento.target.id==='iconoEliminar' || evento.target.id==='iconoMenos'){
                this.mostrarAgregar = false
                const contenido = document.getElementById('agregarGasto')
                contenido.classList.remove('block')
                contenido.classList.add('none')
                this.iconoAgregar='iconoAgregar'
            }
        },
        agregarGasto(){
            alert('elpepe')
        },
        manejoGasto(evento,id){
            if(evento.target.id==='eliminar'){
                this.$emit('eliminarGasto',id)
                alert('elpepe '+id)
            }
            if(evento.target.id==='editar'){
                alert('elpepe '+id)
            }
        }
    }
}
</script>

<style>
.contenido{
    transition: all 0.3s ease;
}
.contenido.none{
    height: 0;
    visibility: hidden;
    overflow: hidden;
}
.contenido.block{
    height: 14rem;
    overflow: hidden;
}
.accion{
    transform: scale(1.5);
}
@media (max-width:576px) {
    .contenido.block{
        height: 20rem;
        overflow: hidden;
    }
}
</style>