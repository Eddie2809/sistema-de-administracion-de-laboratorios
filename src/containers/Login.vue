<script>
    /*
        Asignado a: Erick 

        Notas:
        -Las imágenes necesarias las puedes importar de assets

        Funciones útiles:
        -evaluateCredentials
    */

    export default {
        data(){
            return {
                username: "",
                password: ""
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