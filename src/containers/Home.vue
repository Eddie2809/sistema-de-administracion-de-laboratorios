<script>
    import Calendar from '../components/Calendar.vue'

    export default {
        props: ['getReservations'],
        components: {
            Calendar
        },
    }
/* 
    Asignado a: Rudygei    

    Funciones útiles:
    -getEvents
*/
</script>

<template>
    <div class="container Home">
        <Calendar :events="[{}]"/>
    </div>
</template>