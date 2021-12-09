<template>

    <div v-if="loaded" class="information">
        <h1>Informe lector anual</h1>
        <h2>Nombre: <span>{{name}}</span></h2>
        <h2>Libros leídos: <span>{{balance}} este año </span></h2>
        <h2>Libros leídos: <span>{{balance}} este mes</span></h2>
        <h2>Libros pendientes por leer: <span>{{balance}} ¡Ánimo!</span></h2>
    </div>

</template>


<script>
import jwt_decode from "jwt-decode";
import axios from 'axios';

export default {
    name: "Account",

    data: function(){
        return {
            name: "",
            email: "",
            balance: 0,
            loaded: false,
        }
    },

    methods: {
        getData: async function () {

            if (localStorage.getItem("token_access") === null || localStorage.getItem("token_refresh") === null) {
                this.$emit('logOut');
                return;
			}

            await this.verifyToken();
            
            let token = localStorage.getItem("token_access");
            let userId = jwt_decode(token).user_id.toString();
            
            axios.get(`https://torinporin.herokuapp.com/user/${userId}/`, {headers: {'Authorization': `Bearer ${token}`}})
                .then((result) => {
                    this.name = result.data.name;
                    this.email = result.data.email;	
                    this.balance = result.data.account.balance;
                    this.loaded = true;
                    })
                .catch(() => {
                    this.$emit('logOut');
                });
        },

        verifyToken: function () {
            return axios.post("https://torinporin.herokuapp.com/refresh/", {refresh: localStorage.getItem("token_refresh")}, {headers: {}})
				.then((result) => {
					localStorage.setItem("token_access", result.data.access);		
				})
				.catch(() => {
					this.$emit('logOut');
				});
        }
    },

    created: async function(){
        this.getData();
    },
}
</script>


<style>
    .information{
        padding: 0%;
        width: 100%;
        height: 100%;

        display: flex;
        flex-direction: column;
        justify-content: right;    
        align-items: right;
        background: rgb(2,0,36);
        background: linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(9,9,121,1) 35%, rgba(0,212,255,1) 100%); 
    }

    .information h1{
        font-size: 60px;
        color: #ffffff;
        font-size: 2rem; 
        text-align: left;
        margin-left: 15px;
        font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
    }

    .information h2{
        font-size: 1.7rem; 
        color: #ffffff;
        margin-left: 15px;
        font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    }

    .information span{
        font-size: 1.7rem;
        color: rgb(255, 255, 255);
        font-weight: bold;
        margin-left: 15px;
        font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    }
</style>