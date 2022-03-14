<template>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
    <div class="imagenes">
        <a href="#" class="box">
            <div class="inicio">Inicio</div>
            <div class="home"><i class="fa fa-home"></i></div>
        </a>
    </div>
    <form class="user">
        <div class="arriba">
            <h1 class="titulo3">Registro</h1>
        </div>
        <div class="signUp_user">
            <div class="container_signUp_user">
                <form v-on:submit.prevent="processSignUp" >
                    <p class="text">Nombre:</p>
                    <input type="text" class="input" v-model="user.nombre" placeholder="Ingrese su nombre">
                    <br>

                    <p class="text" >Apellido:</p>
                    <input type="text" class="input" v-model="user.apellido" placeholder="Ingrese su apellido">
                    <br>

                    <p class="text"  >Celular:</p>
                    <input type="text" class="input" v-model="user.celular" placeholder="Ingrese su # de celular">
                    <br>

                    <p class="text">Email:</p>
                    <input type="email" class="input" v-model="user.email" placeholder="Ingrese su email">
                    <br>  

                    <p class="text">Contraseña:</p>
                    <input type="password" class="input" v-model="user.password" placeholder="Ingrese una contraseña">
                    <br>
                    
                    <div class="base">
                    <button type="submit" class="boton">Registrarme</button>
                    </div>
                </form>
            </div>
        </div>
    </form>
    <div class="redes">
        <a href="#" class="facebook"><i class="fa fa-facebook"></i></a>
        <a href="#" class="twitter"><i class="fa fa-twitter"></i></a>
        <a href="#" class="instagram"><i class="fa fa-instagram"></i></a>
    </div>

</template>




<script>
import axios from 'axios';

export default {
    name: "SignUp",

    data: function(){
        return {
            user: {
                nombre: "",
                apellido: "",
                celular: "",
                email: "",
                password: "",
            }
        }
    },

    methods: {
        processSignUp: function(){
            axios.post(
                "https://talentohumanounal.herokuapp.com/user/", 
                this.user,  
                {headers: {}}
            )
                .then((result) => {
                    let dataSignUp = {
                        email: this.user.email,
                        token_access: result.data.access,
                        token_refresh: result.data.refresh,
                    }
                    
                    this.$emit('completedSignUp', dataSignUp)
                })
                .catch((error) => {
                    console.log(error)
                    alert("Error en el resgistro.");
                    
                    
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

.user{
    width: 500px;
    height: 520px;
    background: white;
    margin: auto;
    margin-top: 10px;
    box-shadow: 7px 13px 37px #000;
    
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

.box{
    display: flex;
    flex-direction: row-reverse;
    margin-left: 850px;
    text-decoration:none;
}

.redes{
    margin-top: 60px;
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

.titulo3{
    background: #666;
    color: white;
    text-align: center;
    padding: 15px;
    border: black;
}

.arriba{
    margin-bottom: 20px;
}

.text{
    margin-left: 20px;
    font-size: 20px;
    margin-top: 10px;
}

.input{
    margin-left: 20px;
    font-size: 20px;
    margin-top: 5px;
    width: 90%;
}

.base{
    margin-top: 20px;
    text-align: center;
}

.boton{
    text-align: center;
    font-size: 30px;
    margin-top: 10px;
    padding: 7px;
    background:#94b43b;
    color: white;
    border: #94b43b ;
    border-radius: 5px;
}
</style>