<script>
    export default {
        data(){
            return {
                username: "eddvar",
                password: "o8OR1e8HYo"
            }
        },
        props: ['evaluateCredentials']
    }
</script>

<template>
    <div>
        Usuario:
        <input v-model="username" type="text"><br>
        Contraseña:  
        <input v-model="password" type="password"><br>
        <button @click="this.evaluateCredentials(this.username,this.password)">Ingresar</button>

    </div>
</template>