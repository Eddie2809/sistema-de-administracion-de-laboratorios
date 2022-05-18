<script>
/* 
    Asignado a: Oscar

    RESTRICCIONES A TOMAR EN CUENTA:

    X-No puedes enviar una solicitud sin al menos una reservación
    X-Consultar reservaciones en fines de semana
    X-Hora de inicio empieza a partir de las 7 y hora de fin consultar con el cliente
    X-Verificar disponibilidad de laboratorio IMPORTANTE
    X-No puedes reservar por el resto del semestre si tienes especificado un número de semanas y vice versa
    -En el calendario inferior ir añadiendo los eventos a modo de vista previa con color diferente
    -Mostrar los eventos del laboratorio escogido en el calendario inferior
    -Cada evento tiene que ser único por día. Es decir, si se seleccionó lunes (25 de abril) de 5 a 6 por 3
    semanas, vas a añadir tres eventos, uno el 25 de abril, otro el 2 de mayo y otro el 9 de mayo.
    -Que no se sobreponga con otros eventos

    Funciones útiles

    -getEvents
    -createNewReservation
*/
    import Calendar from '../components/Calendar.vue'

    let periods = [
        {
            name: 'Primavera',
            start: '01-17',
            //end: '05-24'
            end: '06-24'
        },
        {
            name: 'Verano',
            //start: '05-30',
            start: '07-08',
            end: '07-09'
        },
        {
            name: 'Otoño',
            start: '08-15',
            end: '12-15'
        }
    ]

    export default{
    props: ["getLabs", "labsList","getEvents","events","createNewReservation",'getEventsAsync'],
    mounted() {
        this.getLabs()
    },
    components: { Calendar },
    data(){
        return({
            labId: -1,
            weeks: 1,
            allSemester: false,
            reason: '',
            days: [],
            hora: '',
            disabledWeeks: false,
        })
    },
    watch: {
        labId(){
            this.getEvents(this.labId)
        },
    },
    methods: {
        sendRequest(){
            if(this.days.length === 0){
                alert('Agrega al menos un día')
                return
            }
            if(this.weeks <= 0 && this.allSemester === false){
                alert('Número inválido de semanas')
                return
            }

            let primaveraStart = new Date(new Date().getFullYear() + '-' + periods[0].start + 'T00:00:00')
            let primaveraEnd = new Date(new Date().getFullYear() + '-' + periods[0].end + 'T00:00:00')
            let veranoStart = new Date(new Date().getFullYear() + '-' + periods[1].start + 'T00:00:00')
            let veranoEnd = new Date(new Date().getFullYear() + '-' + periods[1].end + 'T00:00:00')
            let otonoStart = new Date(new Date().getFullYear() + '-' + periods[2].start + 'T00:00:00')
            let otonoEnd = new Date(new Date().getFullYear() + '-' + periods[2].end + 'T00:00:00')
            let today = new Date()
            let upperBound,dayObj
            let daysObj = []
            let hours = []
            let weekCount,inicio,final
            let firstDayOfWeek = this.getFirstDayOfWeek()

            //Crear arreglo daysObj de los días seleccionados
            for(let i = 0; i < this.days.length; i++){
                dayObj = this.createDayObj(firstDayOfWeek,parseInt(this.days[i].day))
                daysObj.push({
                    inicio: new Date(dayObj.toISOString().slice(0,11) + this.days[i].start),
                    final: new Date(dayObj.toISOString().slice(0,11) + this.days[i].end)
                })
            }
            
            if(today >= primaveraStart && today <= primaveraEnd) upperBound = primaveraEnd
            if(today >= veranoStart && today <= veranoEnd) upperBound = veranoEnd
            if(today >= otonoStart && today <= otonoEnd) upperBound = otonoEnd

            for(let i = 0; i < daysObj.length; i++){
                if(daysObj[i].inicio > daysObj[i].final){
                    alert('La hora de inicio tiene que ser previa a la hora de final')
                    return
                }
                weekCount = 0
                inicio = daysObj[i].inicio
                final = daysObj[i].final
                while(final < upperBound){
                    if(!this.allSemester && weekCount >= this.weeks){
                        break
                    }
                    hours.push({
                        inicio: inicio,
                        final: final
                    })
                    weekCount++
                    inicio = this.createDayObj(inicio,7)
                    final = this.createDayObj(final,7)
                }
            }

            this.createNewReservation(this.reason,this.labId,hours)
        },
        createDayObj(dayObj,ndays){
            return new Date(dayObj.getTime() + (1000 * 60 * 60 * 24 * ndays))
        },
        getFirstDayOfWeek(){
            let today = new Date()
            let weekday = today.getDay()
            if(weekday === 0) weekday = 6
            else weekday = weekday - 1
            return new Date(today.getTime() - (1000 * 60 * 60 * 24 * weekday))
        },
        addNewDay(){
            if(this.days.length === 7){
                alert('Ya no puedes agregar más días')
                return
            }
            this.days.push({day: '0',start: '00:00',end: '00:00'})
        },
        checkForAvailability(lab){
            if(lab.disponibilidad === false){
                alert('Este laboratorio no está disponible por el momento')
                this.labId = "-1"
            } 
        },
        checkForOverlapping(){

        },
    },
    computed: {
        hideBorder(){
            if(this.days.length === 0) return ''
            else return 'hours'
        }
    }
}
</script>

<template>
    <div class="container ReservationRequest">
        <p class="title">Realiza una solicitud de reservación</p>
        <div class="reservationData">
            <div class="s1">
                <div class="labInput">
                    <p>Laboratorio: </p>
                    <select v-model="this.labId">
                        <option selected value="-1">--Elige un laboratorio--</option>
                        <option @click="checkForAvailability(labs)" v-for="labs in labsList" :key="labs.id_laboratorio" :value="labs.id_laboratorio">{{labs.nombre_laboratorio}}</option>
                    </select>
                </div>
                <p class="subtext">Nota: Puedes ver en el horario de abajo una vista previa</p>
            </div>
            <div class="s2">
                <div class="weeksInput">
                    <p>Cantidad de semanas: </p>
                    <input :disabled="this.disabledWeeks" v-model="this.weeks" min="1" type="number">
                </div>
                <div class="semesterInput">
                    <input @click="this.disabledWeeks = !this.disabledWeeks; this.weeks = 0" v-model="this.allSemester" type="checkbox">
                    <p>Reservar por el resto del periodo</p>
                </div>
            </div>
            <div class="s3">
                <p>Razón (opcional): </p><br>
                <textarea maxlength="300" v-model="this.reason" style="resize: none"></textarea>
            </div>
        </div>
        <div :class="hideBorder">
            <div v-for="day in days" class="row">
                <div class="day">
                    <p>Día</p>
                    <select v-model="this.days.filter(val => val === day)[0].day">
                        <option value="0">Lunes</option>
                        <option value="1">Martes</option>
                        <option value="2">Miércoles</option>
                        <option value="3">Jueves</option>
                        <option value="4">Viernes</option>
                        <option value="5">Sábado</option>
                        <option value="6">Domingo</option>
                    </select>
                </div>
                <div class="start">
                    <p>Hora de inicio:</p>
                    <input v-model="this.days.filter(val=> val === day)[0].start" type="time">
                </div>
                <div class="end">
                    <p>Hora de fin:</p>
                    <input v-model="this.days.filter(val => val === day)[0].end" type="time">
                </div>
                <svg title="Borrar esta fila" @click="this.days = this.days.filter(val => val !== day)" xmlns="http://www.w3.org/2000/svg" width="33" height="33" fill="white" class="bi bi-trash" viewBox="0 0 16 16">
                    <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
                    <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
                </svg>
            </div>
        </div>
        <div @click="this.addNewDay" class="newReservation">
            <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" fill="currentColor" class="bi bi-plus-circle" viewBox="0 0 16 16">
                <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
                <path d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"/>
            </svg>
            <i>Añadir otro día</i>
        </div>
        <button @click="this.sendRequest" class="sendButton">Enviar</button>
        <p class="subtitle">Vista previa</p>
        <Calendar :getEventsAsync="this.getEventsAsync" :events="this.events"/>
    </div>
</template>