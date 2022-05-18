<script>
    /* 
        Asignado a: Bryan

        Notas:
        -Reservaciones pendientes son aquellas con estado 2
        -Reservaciones activas con aquellas con estado 1
        -Reservaciones inactivas son aquellas con estado 0

        Funciones útiles:
        -getReservations
    */
   import Table from '../components/Table.vue'
   
    export default{
        data(){
            return{
                input: ''
            }
        },
        computed: {
            filteredItems() {
                return this.reservations.filter((i) =>{
                    return i.nombre_laboratorio.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.nombreSolicitante.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.apellidoSolicitante.toLowerCase().indexOf(this.input.toLowerCase()) != -1;
                });
            }
        },
        props: ['getReservations', 'userData', 'reservations', 'cancelReservation'],
        mounted() {
            this.getReservations(-1, this.userData.id)
        },
        components: {
            Table
        }
    }
</script>

<template>
    <div class="container MyReservations">
        <div class="body-style">
            <div class="Titulo">
                <h1>Mis reservaciones</h1>
                <h3>Bienvenid@: {{userData.nombre}}</h3>
            </div>

            <div class="search-style">
                <h2>Solicitudes Pendientes</h2>
                <p>Buscar:</p>
                <input v-model="input" type="text" placeholder="Nombre de docente">
            </div>

            <Table :reservations = reservations :reservationStatus = 2 :tableHeaderType = 1 :tableBodyType = 0 :cancelReservation="cancelReservation" />

            <div class="search-style">
                <h2>Reservaciones activas</h2>
                <p>Buscar:</p>
                <input v-model="input" type="text" placeholder="Nombre de docente">
            </div>
            
            <Table :reservations = reservations :reservationStatus = 1 :tableHeaderType = 1 :tableBodyType = 0 :tableButtonType = 1 :cancelReservation="cancelReservation" />

        </div>
    </div>
</template>