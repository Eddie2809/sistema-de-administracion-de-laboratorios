<script>
    /*userData
    apellido: "Duran Cortes"
    contrasena: "cE9FEJ8nRO"
    correo: "190300379@ucaribe.edu.mx"
    id: 5
    nombre: "Bryan Julian"
    nombre_de_usuario: "brydur"
    tipo: "encargado"*/

    /*reservations
    datosReservacion:
    apellidoSolicitante: "Vargas Mendoza"
    completado: false
    estado: 2
    fechaPeticion: "2022-04-20T04:58:01.000Z"
    idLaboratorio: 1
    idReservacion: 5
    idSolicitante: 1
    motivoRechazo: null
    nombreLaboratorio: "Laboratorio de Ingeniería de Software"
    nombreSolicitante: "Eddie Alejandro"
    razonSolicitud: "hace falta pa"*/

    /*Lablist
    apellido_encargado: "Duran Cortes"
    correo_encargado: "190300379@ucaribe.edu.mx"
    id_encargado: 5
    id_laboratorio: 2
    nombre_encargado: "Bryan Julian"
    nombre_laboratorio: "Laboratorio de*/

    /* 
        Asignado a: Bryan

        Notas: 
        -Abrir ventana de confirmación para cancelar reservación
        -Abrir ventana para escribir motivo de rechazo al rechazar una reservación
        -La vista previa es para que el encargado vea como quedaría el horario si aceptara esa reservación
        -En 'Estado' puedes permitir o rechazar las solicitudes de reservación
        -Reservaciones activas son aquellas con estado 1
        -Reservaciones inactivas son aquellas con estado 0
        -Esta pendiente la función para obtener las reservaciones :(

        Funciones útiles:
        -evaluateReservation
        -getReportData
        -cancelReservation
        -getReservations
    */
    import moment from 'moment'
    import Table from '../components/Table.vue'
   
    export default{
        data(){
            return{
                id_lab: ''
            }
        },
        computed: {
            getLab() {
                return this.labsList.filter((i) =>{
                    if(i.id_encargado == this.userData.id){
                        this.id_lab = i.id_laboratorio
                        return i.id_encargado;
                    }
                });
            },
            getWeeks(hours){

            }
        },
        props: ['getReservations', 'userData', 'labsList', 'reservations', 'getLabs'],
        mounted() {
            this.getReservations(this.id_lab, -1)
            this.getLabs()
        },
        components: {
            Table
        },
        methods: {
            moment
        }
    }
</script>

<template>
    <div class="container ManageReservations">
        <div class="body-style">
            <div class="Titulo">
                <h1>Solicitudes de Reservación</h1>
                <button @click="">Generar reporte</button>
            </div>

            <div class="Lab_Options">
                <h3>Bienvenid@: {{userData.nombre}}</h3>
                <h3 v-for="item in getLab" :key="item.id_laboratorio">Laboratorio asignado: {{item.nombre_laboratorio}}</h3>
                <div class="Lab_Status">
                <p>Estado: </p>
                    <select>
                        <option>Se permiten solicitudes</option>
                        <option>No se permiten solicitudes</option>
                    </select>
                    <button>Actualizar</button>
                </div>
            </div>

            <div class="search-style">
                <h2>Solicitudes Pendientes</h2>
                <p>Buscar:</p>
                <input v-model="search" type="text" placeholder="Nombre de docente">
            </div>

            <Table :reservations = reservations :reservationStatus = 2 :tableHeaderType = 0 :tableBodyType = 0 :tableButtonType = 0 />

            <div class="search-style">
                <h2>Reservaciones activas</h2>
                <p>Buscar:</p>
                <input v-model="search" type="text" placeholder="Nombre de docente">
            </div>
            
            <Table :reservations = reservations :reservationStatus = 1 :tableHeaderType = 0 :tableBodyType = 0 :tableButtonType = 1 />

        </div>
    </div>
</template>