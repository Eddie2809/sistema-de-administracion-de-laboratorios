<script>
    //FullCalendar
    import '@fullcalendar/core/vdom' // solves problem with Vite
    import FullCalendar from '@fullcalendar/vue3'
    import timeGridPlugin from '@fullcalendar/timegrid'
    import interactionPlugin from '@fullcalendar/interaction'
    import esLocale from '@fullcalendar/core/locales/es'

    export default{
        components: {
            FullCalendar
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
                    height: 450,
                    displayEventTime: false,
                    events: [
                        {
                            title: 'Matemáticas',
                            start: '2022-04-15T13:00:00',
                            end: '2022-04-15T14:30:00'
                        }
                    ]
                }
            }
        }
    }
</script>

<template>
    <div class="calendarContainer">
        <FullCalendar :options="calendarOptions" />
    </div>
</template>

<style>
    .calendarContainer{
        width: 80%;
        border: 1px solid black;
        padding: 20px;
        border-radius: 10px;
    }
</style>
