<template>

    
    <div class="logIn_user">
        <div class="container_logIn_user">
            <h2>Iniciar sesión</h2>

            <form v-on:submit.prevent="processLogInUser" >
                <input type="text" v-model="user.username" placeholder="Pseudónimo">
                <br>
                <input type="password" v-model="user.password" placeholder="Código de acceso">
                <br>
                <button type="submit">Iniciar Sesion</button>
            </form>
        </div>

    </div>

</template>




<script>
import axios from 'axios';

export default {
    name: "LogIn",

    data: function(){
        return {
            user: {
                username:"",
                password:""
            }
        }
    },

    methods: {
        processLogInUser: function(){
            axios.post(
                "https://torinporin.herokuapp.com/login/", 
                this.user,  
                {headers: {}}
                )
                .then((result) => {
                    let dataLogIn = {
                        username: this.user.username,
                        token_access: result.data.access,
                        token_refresh: result.data.refresh,
                    }
                    
                    this.$emit('completedLogIn', dataLogIn)
                })
                .catch((error) => {
                    
                    if (error.response.status == "401")
                        alert("ERROR 401: Credenciales Incorrectas.");
                    
                });
        }
    }
}
</script>






<style>

    .logIn_user{
        margin: 0;
        padding: 0%;
        height: 100%;
        width: 100%;
    
        display: flex;
        justify-content: center;
        align-items: center;
        background: rgb(2,0,36);
        background: linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(9,9,121,1) 35%, rgba(0,212,255,1) 100%);
    }

    .container_logIn_user {
        border: 10px solid  #3C91E6;
        border-radius: 50%;
        width: 25%;
        height: 70%;
        
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        background-color: cadetblue;
    }

    .logIn_user h2{
        color: #283747;
        font-family:Georgia, 'Times New Roman', Times, serif;
    }

    .logIn_user form{
        width: 60%;
        
    }

    .logIn_user input{
        height: 40px;
        width: 100%;
        border: 8px solid blue;

        box-sizing: border-box;
        padding: 10px 20px;
        margin: 5px 0;

        border: 1px solid #283747;
    }

    .logIn_user button{
        width: 100%;
        height: 40px;

        color: #b6bdc4;
        background: #0B1D51;
        border: 1px solid #E5E7E9;

        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px 0;
    }

    .logIn_user button:hover{
        color: #E5E7E9;
        background: #3C91E6;
        border: 1px solid #283747;
    }

</style>