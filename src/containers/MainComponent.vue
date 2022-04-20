<script>
    import Calendar from '../components/Calendar.vue'

    const apiURL = 'https://salunicaribe-api.herokuapp.com/'

    export default{
        data(){
            return{
                route: {
                    login: true,
                    home: false
                },
                userData: {},
                loggedIn: false,
                events: [{
                    title: 'Fernando Gómez',
                    start: '2022-04-19T12:00',
                    end: '2022-04-19T14:00'
                }]

            }
        },
        components: {
            Calendar
        },
        methods: {
            async fetchData(route,bodyObject){
                const resp = await fetch(apiURL + route, {
                    method: 'post',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify(bodyObject)
                }).then(res => res.json())
                return resp
            },
            async fetchGet(route){
                const resp = await fetch(apiURL + route, {
                    method: 'get',
                    headers: {'Content-Type': 'application/json'}
                }).then(res => res.json())
                return resp
            },

            createNewReservation(reason,labId,hours){
                this.fetchData('new-reservation',{
                    reason: reason,
                    userId: this.userId,
                    labId: labId,
                    hours: hours
                })
                .then(() => alert('Hecho'))
                .catch(err => {
                    alert('Algo salió mal')
                })
            },
            addNewLab(name,labManager){
                this.fetchData('add-new-lab',{
                    name: name,
                    user: labManager
                })
                .then(() => alert('Hecho'))
            },
            addNewUser(name,lastname,email,usertype){
                this.fetchData('user-signup',{
                    name: name,
                    lastname: lastname,
                    email: email,
                    usertype: usertype
                })
                .then(() => alert('Hecho'))
            },
            getEvents(labId){
                this.fetchData('get-events',{labId}).then(res => console.log(res))
            },
            evaluateReservation(reservationId,response,msg){
                this.fetchData('evaluate-reservation',{
                    newStatus: response,
                    reservationId: reservationId,
                    msg: msg
                })
                .then(() => alert('Hecho'))
            },
            evaluateCredentials(username,password){
                this.fetchData('login',{username: username,password: password}).then(res => {
                    if(res === 'Credenciales inválidas'){
                        alert(res)
                    }
                    else{
                        this.route = 'home'
                        this.userData = res
                        console.log(res)
                        alert('Has ingresado')
                    }
                })
            },

        }
    }
</script>

<template>
    <div class="MainComponent">
        <button @click="createNewReservation('hace falta pa',1,[{inicio: '2022-04-19T12:00',final:'2022-04-19T13:00'},{inicio: '2022-04-20T14:00',final: '2022-04-20T16:00'}])">Create reservation</button>
        <button @click="addNewLab('Laboratorio de Mecánica',5)">Add lab</button>
        <button @click="addNewUser('Jon','Doe','jd@gmail.com',3)">Add user</button>
        <button @click="getEvents(2)">Get events</button>
        <button @click="evaluateReservation(6,1,'')">Evaluate reservation</button>
        <button @click="evaluateCredentials('eddvar','o8OR1e8HYo')">Log in</button>
    </div>
</template>

<style>
</style>
