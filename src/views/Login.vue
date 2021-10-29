<template>
    <div class="container">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Page de connexion</h1>

                <form @submit.prevent="submitLoginForm">
                    <div class="field">
                        <label>Email</label>
                        <div class="control">
                            <input type="email" name="email" class="input" v-model="username">
                        </div>
                        <div class="notification is-danger mt-1" v-if="usernameErrors.count">
                                {{usernameErrors.message}}
                        </div>
                    </div>
                    <div class="field">
                        <label>Mot de passe</label>
                        <div class="control">
                            <input type="password" name="password" class="input" v-model="password">
                        </div>
                         <div class="notification is-danger mt-1" v-if="passwordErrors.count">
                            {{passwordErrors.message}}
                        </div>
                    </div>
                    <div class="field">
                        <div class="control">
                            <button class="button is-success">Valider</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
import {toast} from 'bulma-toast';

export default {
    name: 'LogIn',
    data() {
        return {
            username: '',
            password: '',
            usernameErrors: {count: 0, message: 'L\'adresse email est obligatoire' },
            passwordErrors: {count: 0, message:'Le mot de passe est obligatoire' },
            errorsCount: 0,
            errors: [],
        }
    },
    methods: {
        submitLoginForm(){
            console.log("Logging in")
            axios.defaults.headers.common['Authorization'] = ''
            localStorage.removeItem('token')

            this.usernameErrors.count = this.passwordErrors.count = this.errorsCount = 0;
            if(this.username === ''){
                this.usernameErrors.count++
                this.errorsCount++
            }
            if(this.password === ''){
                this.passwordErrors.count++
                this.errorsCount++
            }
            if(this.errorsCount == 0){
                const formData = {
                    username: this.username,
                    password: this.password
                }
                console.log("Form data : " + formData)
                axios
                    .post('/api/v1/token/login/', formData)
                    .then(response=>{
                        const token = response.data.auth_token

                        this.$store.commit('setToken', token)

                        axios.defaults.headers.common['Authorization'] = 'Token ' + token
                        localStorage.setItem('token', token)

                        toast({
                            message: `Vous êtes connecté avec l'adresse ${this.username}`,
                            type: 'is-success',
                            dismissible: true,
                            pauseOnHover: true,
                            duration: 2000,
                            position: 'bottom-right'
                        })
                        this.$router.push('/my-account')
                    })
                    .catch(
                        error=>{
                            this.errors = []
                            if(error.response){
                                for(const property in error.response.data){
                                    toast({
                                        message: `${error.response.data[property][0]}`,
                                        type: 'is-danger',
                                        dismissible: true,
                                        pauseOnHover: true,
                                        duration: 2000,
                                        position: 'bottom-right'
                                    })
                                }
                            } else if(error.message){
                                this.errors.push('Un problème est survenu, réessayez plus tard');

                            }
                        }
                    )
            }
        }
    },
}
</script>
<style scoped>

</style>