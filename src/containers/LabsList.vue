<script>
    /* 
        Asignado a: Eduardo bb 

        Funciones útiles:
        -getLabs
    */
let id = 0

export default {
    data() {
        return {
            input: ''
        }
    },
    computed: {
        filteredItems() {
            return this.list.filter((i) =>{
                return i.nombre_laboratorio.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.nombre_encargado.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.apellido_encargado.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.correo_encargado.toLowerCase().indexOf(this.input.toLowerCase()) != -1;
            });
        }
    },
    props: ['list', 'getLabs'],
    mounted() {
        this.getLabs()
    }
}
</script>

<template>
    <div class="container LabsList">
        <div class="body-style">
            <h1>Laboratorios</h1>
            <div class="search-style">
                <p>Buscar:</p>
                <input v-model="input" placeholder="Laboratorio, encargado o correo">
            </div>
            
            <div class="grid-lista">
                <h2 class="encabezado-lista">Nombre de laboratorio</h2>
                <h2 class="encabezado-lista">Encargado</h2>
                <h2 class="encabezado-lista">Correo</h2>
            </div>

            <div class="grid-lista" v-for="item in filteredItems" :key="item.id_laboratorio">
                <p >{{item.nombre_laboratorio}}</p>
                <p >{{!item.nombre_encargado && !item.apellido_encargado? "Sin encargado asignado":item.nombre_encargado + " " + item.apellido_encargado}}</p>
                <p >{{!item.correo_encargado? "Sin encargado asignado": item.correo_encargado}}</p>
            </div>
        </div>
    </div>
</template>