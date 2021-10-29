<template>
    <div class="container">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Pad d'inscription</h1>

                <form @submit.prevent="submitSignUpForm">
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
                        <label>Confirmation mdp</label>
                        <div class="control">
                            <input type="password" name="passwordConfirm" class="input" v-model="passwordConfirm">
                        </div>
                        <div class="notification is-danger mt-1" v-if="passwordConfirmErrors.count">
                            <div>
                                {{passwordConfirmErrors.message}}
                            </div>
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
import {toast} from 'bulma-toast'

export default {
    name: 'SignUp',
    data() {
        return {
            username: '',
            password: '',
            passwordConfirm: '',
            usernameErrors: {count: 0, message: 'L\'adresse email est obligatoire' },
            passwordErrors: {count: 0, message:'Le mot de passe est obligatoire' },
            passwordConfirmErrors: {count: 0, message:'Les mots de passes ne sont pas identiques' },
            errorsCount: 0,
            errors: [],
        }
    },
    methods: {
        submitSignUpForm(){
            this.usernameErrors.count = this.passwordErrors.count = this.passwordConfirmErrors.count = this.errorsCount = 0;
            if(this.username === ''){
                this.usernameErrors.count++
                this.errorsCount++
            }
            if(this.password === ''){
                this.passwordErrors.count++
                this.errorsCount++
            }
            if(this.password !== this.passwordConfirm){
                this.passwordConfirmErrors.count++
                this.errorsCount++
            }
            if(this.errorsCount == 0){
                const formData = {
                    username: this.username,
                    password: this.password
                }
                console.log(formData)
                axios
                    .post('/api/v1/users/', formData)
                    .then(response=>{
                        toast({
                            message: 'Votre compte a été créé, veuillez vous connecter',
                            type: 'is-success',
                            dismissible: true,
                            pauseOnHover: true,
                            duration: 2000,
                            position: 'bottom-right'
                        })
                        this.$router.push('/log-in')
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
    label{
        font-weight: 500;
    }
</style>