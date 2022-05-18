<script>
   //FullCalendar
    import '@fullcalendar/core/vdom' // solves problem with Vite
    import FullCalendar from '@fullcalendar/vue3'
    import timeGridPlugin from '@fullcalendar/timegrid'
    import interactionPlugin from '@fullcalendar/interaction'
    import esLocale from '@fullcalendar/core/locales/es'
    
    //recibir getEvents como props y luego usar then para cambiar los eventos

    export default{
        props: ['events','getEventsAsync','labId'],
        emits: ['updateEvents'],
        created(){
            this.$emit('updateEvents',this.updateEvents)
        },
        components: {
            FullCalendar
        },
        methods: {
            updateEvents(events){
                let calendarApi = this.$refs.fullCalendar.getApi()
                let idCon = 1
                let ev

                while(ev = calendarApi.getEventById(idCon++)){
                    ev.remove()
                }

                for(let i = 0; i < events.length; i++){
                    events[i].start = new Date(new Date(events[i].start).getTime() - (1000 * 60 * 60 * 5))
                    events[i].end = new Date(new Date(events[i].end).getTime() - (1000 * 60 * 60 * 5))
                    calendarApi.addEvent(events[i])
                }
            },
        },
        watch: {
            labId(){
                this.getEventsAsync(this.labId).then(res => {
                    this.updateEvents(res)
                })
            }
        },
        data() {
            return {
                calendarOptions: {
                    plugins: [timeGridPlugin,interactionPlugin],
                    initialView: 'timeGridWeek',
                    locale: esLocale,
                    slotMinTime: '07:00:00',
                    slotMaxTime: '23:00:00',
                    allDaySlot: false,
                    dayHeaderFormat: {
                        weekday: 'long', 
                    },
                    slotLabelFormat: {
                        hour: 'numeric',
                        minute: 'numeric'
                    },
                    firstDay: 1,
                    height: 860,
                    displayEventTime: false,
                    events: [] 
                },
            }
        }
    }
</script>

<template>
    <div class="calendarContainer">
        <FullCalendar ref="fullCalendar" :options="calendarOptions" />
    </div>
</template>

<style>
    .calendarContainer{
        width: 100%;
        padding: 20px;
    }
</style>
