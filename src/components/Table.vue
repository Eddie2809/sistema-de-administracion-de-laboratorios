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
                msg: '',
                modalVersion: 0
            };
        },
        computed: {
            getWeeks(date){
                return moment(date.start).diff(moment(date.end), 'days');
            }
        },
        props:['reservations', 'reservationStatus', 'tableHeaderType', 'tableBodyType', 'tableButtonType', 'evaluateReservation', 'cancelReservation'],
        methods: {
            moment,
            showModal() {
                this.isModalVisible = true;
            },
            closeModal() {
                this.isModalVisible = false;
            },
            setModalVersion(newModalVersion){
                this.modalVersion = newModalVersion;
            },
            filterHours(horas){
                let min = new Date(10000000000000)

                for(let i = 0; i < horas.length; i++){
                    if(new Date(horas[i].start) < min){
                        min = new Date(horas[i].start)
                    }
                }
                let firstDayOfWeek = this.getFirstDayOfWeek(min)
                let upperbound = this.createDayObj(firstDayOfWeek,7)

                let filteredHours = []
                for(let i = 0; i < horas.length; i++){
                    if(new Date(horas[i].start) >= firstDayOfWeek && new Date(horas[i].end) <= upperbound){
                        filteredHours.push(horas[i])
                    }
                }
                return filteredHours
            },
            getFirstDayOfWeek(today){
                let weekday = today.getDay()
                if(weekday === 0) weekday = 6
                else weekday = weekday - 1
                return new Date(today.getTime() - (1000 * 60 * 60 * 24 * weekday))
            },
            createDayObj(dayObj,ndays){
                return new Date(dayObj.getTime() + (1000 * 60 * 60 * 24 * ndays))
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
                                <p>Cantidad por semanas: {{ moment(reservation.horas[reservation.horas.length - 1].end).diff(moment(reservation.horas[0].start), 'weeks') }}</p>
                                <p>Reservar por todo el semestre: {{}}</p>
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

            <tbody v-if="tableBodyType == 0" v-for="hours in this.filterHours(reservation.horas)">
                <td>Dia: {{moment(hours.start).format('dddd')}}</td>
                <td>Hora de inicio: {{moment(hours.start).format('LT')}}</td>
                <td>Hora de fin: {{moment(hours.end).format('LT')}}</td>
            </tbody>
            <tbody v-if="tableBodyType == 1" v-for="hours in this.filterHours(reservation.horas)">
                <td>
                    <p>Dia: {{moment(hours.start).format('dddd')}}</p>
                    <p>Hora de inicio: {{moment(hours.start).format('LT')}}</p>
                    <p>Hora de fin: {{moment(hours.end).format('LT')}}</p>
                </td>
                <td>
                    <p>Cantidad por semanas: {{ moment(reservation.horas[reservation.horas.length - 1].end).diff(moment(reservation.horas[0].start), 'weeks') }}</p>
                    <p>Reservar por todo el semestre: {{}}</p>
                </td>
                <td>
                    <p>Razon:</p>
                    <p>{{reservation.datosReservacion.razonSolicitud}}</p>
                </td>
            </tbody>
            
            <div v-if="tableButtonType != null" class="buttons">
                <button v-if="tableButtonType == 0" class="green_button" @click="evaluateReservation(reservation.datosReservacion.idReservacion, 1, '');">Aprobar todo</button>
                <button v-if="tableButtonType == 0" class="red_button" @click="setModalVersion(0); showModal();">Rechazar todo</button>
                <button v-if="tableButtonType == 1" class="cancel_button" @click="setModalVersion(1); showModal();">Cancelar reservacion</button>
            </div>
        
            <Modal v-show="isModalVisible"  @close="closeModal" v-if="modalVersion === 0">
                <template v-slot:header> ¿Estas seguro que quieres rechazar esta solicitud? </template>
                <template v-slot:body>
                    <p>Solicitante: {{reservation.datosReservacion.nombreSolicitante + " " + reservation.datosReservacion.apellidoSolicitante}}</p>
                    <p>Fecha de solicitud: {{moment(reservation.datosReservacion.fechaPeticion).format('DD-MM-YYYY')}}</p>
                    <p>Razon: {{reservation.datosReservacion.razonSolicitud}}</p>
                    <textarea placeholder="Escribe una razon" v-model="msg"></textarea>
                </template>
                <template v-slot:footer>
                    <button type="button_accept" class="btn-green" @click="evaluateReservation(reservation.datosReservacion.idReservacion, 0, this.msg); closeModal();"> Aceptar </button>
                </template>
            </Modal>
            
            <Modal v-show="isModalVisible"  @close="closeModal" v-if="modalVersion === 1">
                <template v-slot:header> ¿Estas seguro que quieres cancelar esta solicitud? </template>
                <template v-slot:body>
                    <p>Solicitante: {{reservation.datosReservacion.nombreSolicitante + " " + reservation.datosReservacion.apellidoSolicitante}}</p>
                    <p>Fecha de solicitud: {{moment(reservation.datosReservacion.fechaPeticion).format('DD-MM-YYYY')}}</p>
                    <p>Razon: {{reservation.datosReservacion.razonSolicitud}}</p>
                </template>
                <template v-slot:footer>
                    <button type="button_accept" class="btn-green" @click="cancelReservation(reservation.datosReservacion.idReservacion); closeModal();"> Aceptar </button>
                </template>
            </Modal>
        </table>
    </div>
</template>