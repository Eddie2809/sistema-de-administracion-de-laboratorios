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
            text: '',
            todos: [
                { laboratorio: "Laboratorio de Ingenieria de Software", nombre: "Yarely Baez López", correo: "ybaez@ucaribe.edu.mx", id: id++},
                { laboratorio: "Laboratorio de Ingenieria de Software", nombre: "Yarely Baez López", correo: "ybaez@ucaribe.edu.mx", id: id++},
                { laboratorio: "Laboratorio de Ingenieria de Software", nombre: "Yarely Baez López", correo: "ybaez@ucaribe.edu.mx", id: id++}
            ]
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
                <input v-model="text" placeholder="Laboratorio, encargado o correo">
            </div>
            
            <div class="grid-lista">
                <h2 class="encabezado-lista">Nombre de laboratorio</h2>
                <h2 class="encabezado-lista">Encargado</h2>
                <h2 class="encabezado-lista">Correo</h2>
            </div>

            <div class="grid-lista" v-for="todo in list" :key="todo.id_laboratorio">
                <p >{{todo.nombre_laboratorio}}</p>
                <p >{{todo.nombre_encargado + " " + todo.apellido_encargado}}</p>
                <p >{{todo.correo_encargado}}</p>
            </div>
        </div>
    </div>
</template>