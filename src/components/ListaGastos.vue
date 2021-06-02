<template>
    <section class="row">
        <div class="col-11 col-md-8 border rounded bg-light mx-auto my-3">
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
        </div>
    </section>
</template>

<script>
export default {
    name: 'ListaGastos',
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

@media (max-width:576px) {
    .contenido.block{
        height: 20rem;
        overflow: hidden;
    }
}
</style>