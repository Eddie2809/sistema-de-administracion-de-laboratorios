<script>
    /*
    -Reservaciones pendientes son aquellas con estado 2
    -Reservaciones activas con aquellas con estado 1
    -Reservaciones inactivas son aquellas con estado 0
    
    tableHeaderType = 0: Cabecera con boton 'Generar vista previa' (ManageReservations)
    tableHeaderType = 1: Cabecera simple (MyReservations)
    tableHeaderType = 2: Cabecera con boton 'Generar vista previa'  (AdminTools)

    tableBodyType = 0: Cuerpo de fechas (ManageReservations y MyReservations)
    tableBodyType = 1: Cuerpo compacto (AdminTools)

    tableButtonType = 0: Botones Aceptar y Rechazar (ManageReservations y AdminTools)
    tableButtonType = 1: Boton Cancelar (ManageReservations, MyReservations y AdminTools)

    Codigo para tabla solicitudes pendientes (AdminTools)
    <Table :reservations = reservations :reservationStatus = 2 :tableHeaderType = 2 :tableBodyType = 1 :tableButtonType = 0 />
    
    Codigo para tabla solicitudes activas (AdminTools)
    <Table :reservations = reservations :reservationStatus = 1 :tableHeaderType = 2 :tableBodyType = 1 :tableButtonType = 1 />
    */
    import moment from 'moment'
    import Modal from '../components/Modal.vue'

    export default{
        data (){
            return {
                isModalVisible: false,
            };
        },
        computed: {
            getWeeks(date){
                return moment(date.start).diff(moment(date.end), 'days');
            }
        },
        props:['reservations', 'reservationStatus', 'tableHeaderType', 'tableBodyType', 'tableButtonType'],
        methods: {
            moment,
            showModal() {
                this.isModalVisible = true;
            },
            closeModal() {
                this.isModalVisible = false;
            }
        },
        components: {
            Modal
        }
    }
</script>

<template>
    <div v-for="reservation in reservations" class="table_container">
        <table v-if="reservation.datosReservacion.estado === reservationStatus" class="table_Reservaciones">
            <thead>
                <tr v-if="tableHeaderType == 1 || tableHeaderType == 0">
                    <th class="table_header1">
                        <p>Genero la solicitud: {{reservation.datosReservacion.nombreSolicitante + " " + reservation.datosReservacion.apellidoSolicitante}}</p>
                        <button v-if="tableHeaderType == 0">Generar vista previa</button>
                    </th>
                    <th class="table_header2">
                        <strong>Razon:</strong>
                        <p>{{reservation.datosReservacion.razonSolicitud}}</p>
                    </th>
                    <th class="table_header3">
                        <ul>
                            <li>
                                <p>Fecha de solicitud: {{moment(reservation.datosReservacion.fechaPeticion).format('DD-MM-YYYY')}}</p>
                                <p>Cantidad por semanas: {{ moment(reservation.horas[0].end).diff(moment(reservation.horas[0].start), 'weeks') }}</p>
                                <p>Reservar por todo el semestre: {{reservation.datosReservacion.estado}}</p>
                            </li>
                        </ul>
                    </th>
                </tr>
                <tr v-if="tableHeaderType == 2">
                    <th>
                        <p>Genero la solicitud: {{reservation.datosReservacion.nombreSolicitante + " " + reservation.datosReservacion.apellidoSolicitante}}</p>
                    </th>
                    <th>
                        <p>Fecha de solicitud: {{moment(reservation.datosReservacion.fechaPeticion).format('DD-MM-YYYY')}}</p>
                    </th>
                    <th>
                        <button>Generar vista previa</button>
                    </th>
                </tr>
            </thead>

            <tbody v-if="tableBodyType == 0" v-for="hours in reservation.horas">
                <td>Dia: {{moment(hours.start).format('dddd')}}</td>
                <td>Hora de inicio: {{moment(hours.start).format('LT')}}</td>
                <td>Hora de fin: {{moment(hours.end).format('LT')}}</td>
            </tbody>
            <tbody v-if="tableBodyType == 1" v-for="hours in reservation.horas">
                <td>
                    <p>Dia: {{moment(hours.start).format('dddd')}}</p>
                    <p>Hora de inicio: {{moment(hours.start).format('LT')}}</p>
                    <p>Hora de fin: {{moment(hours.end).format('LT')}}</p>
                </td>
                <td>
                    <p>Cantidad por semanas: {{}}</p>
                    <p>Reservar por todo el semestre: {{reservation.datosReservacion.estado}}</p>
                </td>
                <td>
                    <p>Razon:</p>
                    <p>{{reservation.datosReservacion.razonSolicitud}}</p>
                </td>
            </tbody>
            
            <div v-if="tableButtonType != null" class="buttons">
                <button v-if="tableButtonType == 0" class="green_button" @click="showModal">Aprobar todo</button>
                <button v-if="tableButtonType == 0" class="red_button" @click="showModal">Rechazar todo</button>
                <button v-if="tableButtonType == 1" class="cancel_button" @click="showModal">Cancelar reservacion</button>
            </div>
        </table>
        
        <Modal v-show="isModalVisible"  @close="closeModal">
            <template v-slot:header> ¿Estas seguro que quieres rechazar esta solicitud? </template>
            <template v-slot:body>
                <p>Solicitante: {{reservation.datosReservacion.nombreSolicitante + " " + reservation.datosReservacion.apellidoSolicitante}}</p>
                <p>Fecha de solicitud: {{moment(reservation.datosReservacion.fechaPeticion).format('DD-MM-YYYY')}}</p>
                <p>Razon: {{reservation.datosReservacion.razonSolicitud}}</p>
                <textarea placeholder="Escribe una razon"></textarea>
            </template>
        </Modal>
    </div>
</template>