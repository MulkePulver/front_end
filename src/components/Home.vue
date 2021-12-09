<template>

    <div class="greetings">
        <h1>¡Bienvenido a nuestro sistema de lectura diaria, <span> {{username}} </span>!</h1>
    </div>

</template>


<script>

export default {
    name: "Home",

    data: function(){
        return {
            username: localStorage.getItem('username') || "none"
        }
    }
}

</script>


<style>
    .greetings{
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

    .greetings h1{
        font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
        font-size: 50px;
        color: #b0b5bd;
    }

    .greetings span{
        color: #000000;
        font-weight: bold;
    }
</style>