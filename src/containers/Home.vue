<script>
    import Calendar from '../components/Calendar.vue'

    export default ({
        props: ['func'],
        components: {
            Calendar
        },
    })
/* 
    Asignado a: Rudygei    

    Funciones útiles:
    -getEvents
*/
</script>

<template>
    <div class="container Home">
        <Calendar :events="[{}]"/>
        <button @click="this.func(5)">Haga click pa</button>
    </div>
</template>