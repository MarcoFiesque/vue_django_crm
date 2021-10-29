<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Mon compte</h1>
            </div>
            <div class="column is-12">
                <button class="button is-danger" @click="logout()">Se déconnecter</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'MyAccount',
    methods: {
        async logout(){
            await axios
                    .post('/api/v1/token/logout/')
                    .then(response=>{
                        console.log("Logged out");
                    })
                    .catch(e=>{
                        console.log(JSON.stringify(e))
                    })
                axios.defaults.headers.common['Authorization'] = ''
                localStorage.removeItem('token')
                this.$store.commit('removeToken')

                this.$router.push('/')
        }
    },
}
</script>