<template>
<!--Este home es el inicio de sesión del usuario y se encuentra con el formulario-->
    <div class="form_user">
        <div class="container_form_user">
            <h2>Formulario de postulación</h2>

            <form v-on:submit.prevent="processFormulario" >
                <div class="field-cargo">
                    <label>Cargo</label>
                     <select v-model="cargo"> 
                        <option v-for="cargo in cargoList" :key="cargo.id" :value="cargo.id">{{ cargo.nombre }}</option>
                    </select>
                </div>

                <br>

                <div class="field-estudio">
                    <label>Nivel de estudio</label>
                     <select v-model="estudio"> 
                        <option v-for="estudio in estudioList" :key="estudio.id" :value="estudio.id">{{ estudio.nombre }}</option>
                    </select>
                </div>

                <br>

                <div class="field-experiencia">
                    <label>Experiencia en años </label>
                    <input v-model="experiencia" type="number">
                </div>

                <br>

                <div class="field-salario">
                    <label>Salario en pesos </label>
                    <input v-model="salario" type="number">
                </div>

                <br>

                <div class="field-contrato">
                    <label>Contrato</label>
                     <select v-model="contrato"> 
                        <option v-for="contrato in contratoList" :key="contrato.id" :value="contrato.id">{{ contrato.nombre }}</option>
                    </select>
                </div>

                <br>

                 <div class="field-disponibilidad">
                    <label>Disponibilidad diaria en horas </label>
                    <input v-model="disponibilidad" type="number">
                </div>               

                 <div class="field-cv">
                    <label>Pegue el link de su cv </label>
                    <textarea v-model="cv"></textarea>

                </div>      

                <button type="submit">Registrarse</button>
            </form>
        </div>

    </div>
</template>


<script>
    import axios from 'axios'
    
    export default {
        name: 'Home',
         data(){
            return{
                cargo: '',
                cargoList: [],
                estudio: '',
                estudioList: [],
                contrato: '',
                contratoList: [],
            }
        },
        formato(){
            return{
                cargo: '',
                estudio: '',
                contrato: '',
            }
        },
        mounted() {
            this.getCargoList()
            this.getEstudioList()
            this.getContratoList()
            this.processingFormulario()
            document.title = 'Creating post | PLACE'
        },
        methods: {
        
            processingFormulario: function(){
                axios.post(
                    "https://talentohumanounal.herokuapp.com/formulario/", 
                    this.cargo, 
                    this.estudio,
                    this.experiencia,
                    this.salario,
                    this.contrato,
                    this.disponibilidad,
                    this.cv,
                    {headers: {}}
                )
                .catch(error => {
                console.log(error)
                })
            },


            getCargoList() {
                axios
                .get('https://talentohumanounal.herokuapp.com/cargo/')
                .then(response => {
                    this.cargoList = response.data
                })
                .catch(error => {
                    console.log(error)
                })
    
            },
            getEstudioList() {
                axios
                .get('https://talentohumanounal.herokuapp.com/estudio/')
                .then(response => {
                    this.estudioList = response.data
                })
                .catch(error => {
                    console.log(error)
                })
    
            },
            getContratoList() {
                axios
                .get('https://talentohumanounal.herokuapp.com/contrato/')
                .then(response => {
                    this.contratoList = response.data
                })
                .catch(error => {
                    console.log(error)
                })
    
            },
            submitForm(){
                this.errors = []
    
                if (this.name === ''){
                    this.errors.push('Title input is requiered.')
                }
    
                if (this.description === ''){
                    this.errors.push('Description input is requiered.')
                }
    
                if (this.price === ''){
                    this.errors.push('Price input is requiered.')
                }
    
                if (!this.errors.length){
                    const config = {
                        headers: {
                        "content-type": "multipart/form-data",
                        },
                    };
                    let formData = new FormData();
                    formData.append("name", this.name);
                    formData.append("category", this.category);
                    formData.append("price", this.price);
                    formData.append("description", this.description);
    
                    axios
                        .post("/api/v1/create-post/", formData, config)
                        .then(response => {
                            toast({
                                message: 'Post created!',
                                type: 'is-success',
                                dismissible: true,
                                pauseOnHover: true,
                                duration: 2000,
                                position: 'bottom-right',
                            })
    
                            this.$router.push('/my-account')
                        })
                        .catch(error =>{
                            if (error.response){
                                for (const property in error.response.data) {
                                    this.errors.push(`${property}: ${error.response.data[property]}`)
                                }
    
                                console.log(JSON.stringify(error.response.data))
                            }else if (error.message){
                                this.errors.push('Something bad happened...')
                                
                                console.log(JSON.stringify(error))
                            }
                        })
                }
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
    }

    .greetings h1{
        font-size: 50px;
        color: #283747;
    }

    .greetings span{
        color: crimson;
        font-weight: bold;
    }


    .form_user{
        margin: 0;
        padding: 0%;
        height: 100%;
        width: 100%;
    
        display: flex;
        justify-content: center;
        align-items: center;
    }


    .form_user h2{
        color: #283747;

    }

    .form_user form{
        width: 70%;
        
    }

    .form_user input{
        height: 40px;
        width: 100%;

        box-sizing: border-box;
        padding: 10px 20px;
        margin: 5px 0;

        border: 1px solid #283747;
    }

    .form_user button{
        width: 100%;
        height: 40px;

        color: #E5E7E9;
        background: #283747;
        border: 1px solid #E5E7E9;

        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px 0 25px 0;
    }

    .form_user button:hover{
        color: #E5E7E9;
        background: crimson;
        border: 1px solid #283747;
    }

</style>