<template>
  <div id="app" class="app">

    <div class="header">

      <h1> Sisdia </h1>
      <nav>
        <button v-if="is_auth" v-on:click="loadHome"> Inicio </button>
        <button v-if="is_auth" v-on:click="loadAccount"> Cuenta </button>
        <button v-if="is_auth" v-on:click="logOut"> Cerrar Sesión </button>
        <button v-if="!is_auth" v-on:click="loadLogIn" > Iniciar Sesión </button>
        <button v-if="!is_auth" v-on:click="loadSignUp" > Registrarse </button>
      </nav>
    </div>
    

    <div class="main-component">
      <router-view  
        v-on:completedLogIn="completedLogIn"
        v-on:completedSignUp="completedSignUp"
        v-on:logOut="logOut"
      >
      </router-view>
    </div>
    

    <div class="footer">
      <h2>Por Andrés, recuperación CICLO 3 Misión TIC</h2>
    </div>

  </div>
</template>




<script>
export default {
  name: 'App',

  data: function(){
      return{
        is_auth: false
      }
  },

  components: {
  },

  methods:{
    verifyAuth: function() {
      this.is_auth = localStorage.getItem("isAuth") || false;
		
			if (this.is_auth == false)
        this.$router.push({ name: "logIn" });
      else
        this.$router.push({ name: "home" });
    },

    loadLogIn: function(){
      this.$router.push({name: "logIn"})
    },

    loadSignUp: function(){
      this.$router.push({name: "signUp"})
    },

    completedLogIn: function(data) {
			localStorage.setItem("isAuth", true);
			localStorage.setItem("username", data.username);
			localStorage.setItem("token_access", data.token_access);
			localStorage.setItem("token_refresh", data.token_refresh);
			alert("Autenticación Exitosa");
			this.verifyAuth();
    },

    completedSignUp: function(data) {
			alert("Registro Exitoso");
			this.completedLogIn(data);
    },

    loadHome: function() {
      this.$router.push({ name: "home" });
    },

    loadAccount: function () {
			this.$router.push({ name: "account" });
		},

    logOut: function () {
			localStorage.clear();
			alert("Sesión Cerrada");
			this.verifyAuth();
		},
  },

  created: function(){
    this.verifyAuth()
  }

}
</script>






<style>

  body{
    margin: 0 0 0 0;
  }

  .header{
    margin: 0%;
    padding: 0;
    width: 100%;
    height: 10vh; 
    min-height: 100px;

    background-color: #04395E;
    border: 4px solid #13070C;
    color:#E5E7E9  ;

    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .header h1{
    margin-left: 20px;
    color: #524245;
    border: 2px solid white;
    border-radius: 33%;
    background: rgb(131,58,180);
    background: linear-gradient(90deg, rgba(131,58,180,1) 0%, rgba(176,177,156,1) 50%, rgba(252,176,69,1) 100%); 
    text-align: center;
    font-family: 'Courier New', Courier, monospace;
    font-size: 4.5rem;
  }

  .header nav {
    height: 100%;
    width: 20%;

    display: flex;
    justify-content: space-around;
    align-items: center;

    font-size: 20px;
  }

  .header nav button{
        color: #b6bdc4;
        background: #0B1D51;
        border: 1px solid #E5E7E9;;

        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px;
  }

  .header nav button:hover{
        color: #E5E7E9;
        background: #3C91E6;
        border: 1px solid #283747;
  }

  
  .main-component{
    height: 75vh;
    margin: 0%;
    padding: 0%;

    background: #FDFEFE ;
  }

 
  .footer{
    margin: 0;
    padding: 0;
    width: 100%;
    height: 10vh;
    min-height: 100px; 

    background: rgb(2,0,36);
    background: linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(9,88,121,1) 35%, rgba(0,212,255,1) 100%); 

  }

  .footer h2{
    width: 100%;
    height: 100%;
    
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
