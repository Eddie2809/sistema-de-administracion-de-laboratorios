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
   import moment from 'moment'
   
    export default{
        data(){
            return{

            }
        },
        computed: {
            getWeeks(hours){

            }
        },
        props: ['getReservations', 'userData', 'reservations'],
        mounted() {
            this.getReservations(-1, this.userData.id)
        },
        methods: {
            moment
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
                <input type="text" placeholder="Nombre de docente">
            </div>

            <div v-for="reservation in reservations" class="table_container">
                <table v-if="reservation.datosReservacion.estado === 2" class="table_Reservaciones">
                    <thead>
                        <tr>
                            <th class="table_header1">
                                <p>Laboratorio solicitado:</p>
                                <p>{{reservation.datosReservacion.nombreLaboratorio}}</p>
                            </th>
                            <th class="table_header2">
                                <strong>Razon:</strong>
                                <p>{{reservation.datosReservacion.razonSolicitud}}</p>
                            </th>
                            <th class="table_header3">
                                <ul>
                                    <li>
                                        <p>Fecha de solicitud: {{moment(reservation.datosReservacion.fechaPeticion).format('DD-MM-YYYY')}}</p>
                                        <p>Cantidad por semanas: {{}}</p>
                                        <p>Reservar por todo el semestre: {{reservation.datosReservacion.estado}}</p>
                                    </li>
                                </ul>
                            </th>
                        </tr>
                    </thead>

                    <tbody v-for="hours in reservation.horas">
                        <td>Dia: {{moment(hours.start).format('dddd')}}</td>
                        <td>Hora de inicio: {{moment(hours.start).format('LT')}}</td>
                        <td>Hora de fin: {{moment(hours.end).format('LT')}}</td>
                    </tbody>
                </table>
            </div>

            <div class="search-style">
                <h2>Reservaciones activas</h2>
                <p>Buscar:</p>
                <input type="text" placeholder="Nombre de docente">
            </div>
                
            <div v-for="reservation in reservations" class="table_container">
                <table v-if="reservation.datosReservacion.estado === 1" class="table_Reservaciones">
                    <thead>
                        <tr>
                            <th class="table_header1">
                                <p>Laboratorio solicitado:</p>
                                <p>{{reservation.datosReservacion.nombreLaboratorio}}</p>
                            </th>
                            <th class="table_header2">
                                <p>Razón:</p>
                                <p>{{reservation.datosReservacion.razonSolicitud}}</p>
                            </th>
                            <th class="table_header3">
                                <ul>
                                    <li>
                                        <p>Fecha de solicitud: {{moment(reservation.datosReservacion.fechaPeticion).format('DD-MM-YYYY')}}</p>
                                        <p>Cantidad por semanas: {{}}</p>
                                        <p>Reservar por todo el semestre: {{reservation.datosReservacion.estado}}</p>
                                    </li>
                                </ul>
                            </th>
                        </tr>
                    </thead>

                    <tbody v-for="hours in reservation.horas">
                        <td>Dia: {{moment(hours.start).format('dddd')}}</td>
                        <td>Hora de inicio: {{moment(hours.start).format('LT')}}</td>
                        <td>Hora de fin: {{moment(hours.end).format('LT')}}</td>
                    </tbody>
                    <div class="buttons">
                        <button class="cancel_button">Cancelar reservacion</button>
                    </div>
                </table>
            </div>
        </div>
    </div>
</template>