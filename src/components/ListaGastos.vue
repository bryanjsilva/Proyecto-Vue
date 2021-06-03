/* Este componente maneja la lista de gastos, las funciones de agregar, editar y eliminar así como el filtro */
<template>
    <section class="row">
        <div class="col-11 border rounded bg-light mx-auto my-3">
            <div class="d-flex justify-content-around m-3">
                <h4 class="align-self-end m-auto">Agregar gasto</h4>
                <button 
                v-bind:id='iconoAgregar'
                v-on:click.prevent="manejoClick($event)"
                class="btn btn-warning m-auto">
                    <i v-show='!mostrarAgregar' id='iconoMas' class="fa fa-plus fa-fw" v-on:click.prevent="manejoClick($event)"></i>
                    <i v-show='mostrarAgregar' id='iconoMenos' class='fa fa-minus fa-fw' v-on:click.prevent="manejoClick($event)"></i>
                </button>
            </div>
            <form id='agregarGasto' class="container mt-3 mb-2 card contenido none">
                <div class="row justify-content-center m-2">
                    <div class="col-12 col-md-5">
                        <label class="form-label" for='nombreGasto'>Nombre del gasto</label>
                    </div>
                    <div class="col-12 col-md-5">
                        <input type="text" name="nombreGasto" id="nombreGasto" class="form-control" v-model='nuevoGasto.nombre'>
                    </div>
                </div>
                <div class="row justify-content-center m-2">
                    <div class="col-12 col-md-5">
                        <label class="form-label" for='montoGasto'>Monto del gasto</label>
                    </div>
                    <div class="col-12 col-md-5">
                        <input type="number" name="montoGasto" id="montoGasto" class="form-control" min='0' v-model='nuevoGasto.monto'>
                    </div>
                </div>
                <div class="row justify-content-center m-2">
                    <div class="col-12 col-md-5">
                        <label class="form-label" for='tipoGasto'>Tipo del gasto</label>
                    </div>
                    <div class="col-12 col-md-5">
                        <select name="tipoGasto" id="tipoGasto" class="form-select" v-model='nuevoGasto.tipo'>
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
                        v-show="!editar"
                        v-on:click.prevent='this.$emit("agregarGasto",nuevoGasto)'
                        class="btn btn-primary w-100">
                            Agregar
                        </button>
                        <button
                        v-show='editar'
                        id='botonEditar'
                        v-on:click.prevent='manejoEdicion($event,nuevoGasto)'
                        class="btn btn-primary w-100">
                            Editar
                        </button>
                    </div> 
                </div>
            </form>
            <section class="row">
                <div class="col-2 col-md-1">
                    <label for="filtro" class="form-label">Filtrar: </label>
                </div>
                <div class="col-8 col-md-3">
                    <select name="filtro" id="filtro" class="form-select" v-model='filtrarGastos'>
                        <option selected value="">Todos los gastos</option>
                        <option value="Entretenimiento">Entretenimiento</option>
                        <option value="Hogar">Hogar</option>
                        <option value="Salud">Salud</option>
                        <option value="Educación">Educación</option>
                        <option value="Otro">Otro</option>
                    </select>
                </div>

            </section>
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
                v-for='(gasto,index) in gastos'
                v-bind:key='index'
                class="row my-3 text-center">
                    <div class="col-4">
                        {{gasto.nombre}}
                    </div>
                    <div class="col-3">
                        ${{gasto.monto}}
                    </div>
                    <div class="col-3">
                        {{gasto.tipo}}
                    </div>
                    <div class="col-2 justify-content-around">
                        <i 
                        v-on:click="manejoEdicion($event,gasto,index)"
                        id='editar'
                        class="fa fa-edit fa-fw accion text-warning m-2"></i>
                        <i 
                        v-on:click.prevent="manejoEliminar({id:gasto.id, indice: index})"
                        id='eliminar'
                        class="fa fa-trash fa-fw accion text-danger m-2"></i>
                    </div>
                </div>
                <div class="row bg-dark text-light h5 justify-content-center m-2 p-2 rounded">
                    <div 
                    class='col-5 m-auto'>
                        Total gastos:
                    </div>
                    <div class="col-5 m-auto">
                        ${{suma}}
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
            iconoAgregar: 'iconoAgregar',
            nuevoGasto: {nombre: '', monto: '', tipo: 'Entretenimiento'},
            suma: 0,
            editar: false,
            gastoID: '',
            filtrarGastos: '',
            gastos: []
        }
    },
    // Antes de que se monte la lista en la pantalla se muestran todos los gastos que tiene el usuario
    beforeMount(){
        this.gastos = []
        this.gastos = this.listaGastos
    },
    // Antes de que se actualice, ya sea al eliminar, editar, agregar un gasto o filtrar, se actualiza la lista que se mostrará en pantalla
    beforeUpdate(){
        this.gastos = []
        this.gastos = this.listaGastos.filter(gasto=>{
            if(this.filtrarGastos===''){
                return this.listaGastos
            }else{
                return gasto.tipo===this.filtrarGastos
            }
        })
        this.suma = 0
        this.gastos.forEach(gasto => {
            this.suma += parseFloat(gasto.monto)
        });
        this.suma = Math.round(this.suma*100)/100
    },
    methods:{
        // Esta función permite expander o retraer el campo para agregar o editar el formulario
        manejoClick(evento){
            if(evento.target.id==='iconoAgregar' || evento.target.id==='iconoMas' || evento.target.id==='editar'){
                this.iconoAgregar='iconoEliminar'
                this.mostrarAgregar = true
                const contenido = document.getElementById('agregarGasto')
                contenido.classList.add('block')
                contenido.classList.remove('none')
            }else if(evento.target.id==='iconoEliminar' || evento.target.id==='iconoMenos' || evento.target.id==='editar'){
                this.mostrarAgregar = false
                const contenido = document.getElementById('agregarGasto')
                contenido.classList.remove('block')
                contenido.classList.add('none')
                this.iconoAgregar='iconoAgregar'
            }
        },
        // Esta función permite editar el gasto seleccionado y se envían los nuevos datos a la app principal a través de $emit
        manejoEdicion(evento, gasto, indice){
            if(evento.target.id==='editar'){
                this.manejoClick(evento)
                this.editar=true
                this.nuevoGasto={nombre:gasto.nombre,monto:gasto.monto,tipo:gasto.tipo}
                this.gastoID={id: gasto.id, index: indice}
            }
            if(event.target.id==='botonEditar'){
                if(this.nuevoGasto.nombre!==''){
                    this.editar=false
                    this.$emit('editarGasto',this.nuevoGasto,this.gastoID)
                    this.nuevoGasto = {nombre: '', monto: '', tipo: 'Entretenimiento'}
                }
            }
        },
        // Esta función envía los datos necesarios para eliminar el gasto seleccionado a través de $emit, además actualiza la lista
        manejoEliminar(dato){
            this.$emit('eliminarGasto',dato)
            this.gastos = []
            this.gastos = this.listaGastos.filter(gasto=>{
            if(this.filtrarGastos===''){
                return this.listaGastos
            }else{
                return gasto.tipo===this.filtrarGastos
            }
        })
        }
    }
}
</script>

<style>
/* Las clases contenido permiten ejecutar una animación al momento de expander o retraer el campo para editar o agregar un gasto */
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

/* Este media query se usa para que el contenido desplegable se vea bien en pantallas móviles, aunque la lista de tareas, no es del todo responsive */
@media (max-width:576px) {
    .contenido.block{
        height: 20rem;
        overflow: hidden;
    }
}
</style>