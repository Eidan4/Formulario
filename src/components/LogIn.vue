<template>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

    <div class="imagenes">
        <a href="#" class="box">
            <div class="inicio">Inicio</div>
            <div class="home"><i class="fa fa-home"></i></div>
        </a>
    </div>

    <div class="logIn_user">
        <div class="container_logIn_user">

            <form class=login>

                <h2 class="titulo">Iniciar sesión</h2>

                <form v-on:submit.prevent="processLogInUser" >
                    <!--email-->
                    <p class="text">Email</p>
                    <input type="text" class="controll" v-model="user.email" placeholder="Ingrese Email">
                    <br>
                    <!--Contraseña-->
                    <p class="text">Contraseña</p>
                    <input type="password" class="controll" v-model="user.password" placeholder="Password">
                    <br>
                    <!--Boton iniciar sesion-->
                    <button class="button" type="submit">Iniciar Sesion</button>
                </form>
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
                email:"",
                password:"",
            }
        }
    },

    methods: {
        processLogInUser: function(){
            axios.post(
                "https://talentohumanounal.herokuapp.com/login/", 
                this.user,  
                {headers: {}}
                )
                .then((result) => {
                    let dataLogIn = {
                        email: this.user.email,
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
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Anonymous Pro Bold Italic', arial;
}

.login{
    width: 400px;
    height: 340px;
    background: white;
    margin: auto;
    margin-top: 40px;
    box-shadow: 7px 13px 37px #000;
    left: 50%;
    top: 50%;
    position: absolute;
    transform: translate(-50%, -50%)
}

.titulo{
    margin: 0;
    height: 40px;
    margin-top: 10px;
    margin-bottom: 10px;
    margin-left: 20px;
    color: #94b43b;
}

.controll{
    width: 90%;
    margin-bottom: 15px;
    padding: 11px 10px;
    margin-left: 20px;
}

.button{
    width: 90%;
    height: 60px;
    border-radius: 10px;
    margin-left: 20px;
    margin-top: 20px;
    font-size: 20px;
    color: white;
    background: #94b43b;
    border: #94b43b;
}

.text{
    margin-left: 20px;
    font-size: 20px;
    margin-bottom: 5px;
}

.box{
    display: flex;
    flex-direction: row-reverse;
    margin-left: 850px;
    text-decoration:none;
}

.imagenes{
    display: flex;
    margin-left: 25px;
    width: 400px;
    margin-top: 10px;
}

.home{
    font-size: 70px;
    margin-top: 30px;
    margin-right: 10px;
    color: #94b43b;
}

.inicio{
    margin-top: 52px;
    font-size: 25px;
    color: black;
}

.redes{
    margin-top: 450px;
    margin-left: 1085px;
    font-size: 45px;
    
}

.instagram{
    margin-left: 40px;
    color: #666;
}

.twitter{
    margin-left: 40px;
    color: #666;
}

.facebook{
    color: #666;
}
</style>