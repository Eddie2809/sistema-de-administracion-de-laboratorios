<script>
   //FullCalendar
    import '@fullcalendar/core/vdom' // solves problem with Vite
    import FullCalendar from '@fullcalendar/vue3'
    import timeGridPlugin from '@fullcalendar/timegrid'
    import interactionPlugin from '@fullcalendar/interaction'
    import esLocale from '@fullcalendar/core/locales/es'

    export default{
        props: ['events'],
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

                console.log(events)

                while(ev = calendarApi.getEventById(idCon++)){
                    ev.remove()
                }

                for(let i = 0; i < events; i++){
                    calendarApi.addEvent(events[i])
                }
            }
        },
        watch: {
            events(){
                let calendarApi = this.$refs.fullCalendar.getApi()
                let idCon = 1
                let ev

                while(ev = calendarApi.getEventById(idCon++)){
                    ev.remove()
                }
                calendarApi.addEvent(this.events[0])
                console.log(this.events)
                for(let i = 0; i < this.events; i++){
                    calendarApi.addEvent(this.events[i])
                }  
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
    <div class="calendarContainer" @click="this.updateEvents()">
        <FullCalendar ref="fullCalendar" :options="calendarOptions" />
    </div>
</template>

<style>
    .calendarContainer{
        width: 100%;
        padding: 20px;
    }
</style>
