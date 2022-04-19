<script>
    import Calendar from '../components/Calendar.vue'

    export default{
        data(){
            return{
                route: 'login',
                loggedIn: false 
            }
        },
        components: {
            Calendar
        },
        methods: {
            async fetchData(route,bodyObject){
                const resp = await fetch('https://salunicaribe-api.herokuapp.com/' + route, {
                    method: 'post',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify(bodyObject)
                }).then(res => res.json())
                return resp
            },
            async fetchGet(route){
                const resp = await fetch('https://salunicaribe-api.herokuapp.com/' + route, {
                    method: 'get',
                    headers: {'Content-Type': 'application/json'}
                }).then(res => res.json())
                return resp
            }
        }
    }
</script>

<template>
    <div class="MainComponent">
        <Calendar/>
    </div>
</template>

<style>
</style>
