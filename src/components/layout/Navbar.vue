<template>
    <nav class="navbar is-dark">
        <div class="navbar-brand">
            <router-link to="/" class="navbar-item">
                <strong>Django-Vue CRM</strong>
            </router-link>
        </div>
        <div class="navbar-menu">
            <div class="navbar-end">
                <div class="navbar-item">
                    <div class="buttons">
                        <router-link class="button is-success" to="/sign-up">
                            <strong>Inscription</strong>
                        </router-link>
                        <router-link class="button is-light" to="/log-in">
                            <strong>Connexion</strong>
                        </router-link>
                    </div>
                </div>
            </div>
        </div>
    </nav>
</template>

<script>
    export default {
        name: 'Navbar'
    }
</script>