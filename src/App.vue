<template>
  <div>
    <Navbar/>
    <section class="section">
      <router-view/>
    </section>
  </div>
</template>

<script>
import Navbar from '@/components/layout/Navbar'
import axios from 'axios'

axios.defaults.baseURL = 'http://127.0.0.1:8000'


export default {
  name: 'App',
  components: {
    Navbar
  },
  beforeCreate() {
    this.$store.commit('initalizeStore')

    if(this.$store.state.token){
      axios.defaults.headers.common['Authorization'] = "Token " + this.$store.state.token
    } else {
      axios.defaults.headers.common['Authorization'] = ''
    }
  },
}
</script>

<style lang="scss">
  @import '../node_modules/bulma';
</style>
