<template>
  <div id="app">
    <NavBar :status="loggedStatus" v-on:loggedin="changeStatus"></NavBar>
    <Error v-if="error_status" v-bind:error="error_msg"/>
    <router-view v-on:loggedin="changeStatus"/>
  </div>
</template>

<script>
import axios from 'axios';
import NavBar from '@/components/NavBar.vue'
import Error from '@/components/Error.vue';
import {mapState} from 'vuex'

export default {
  components: {
    NavBar,
    Error,
  },
  data () {
    return {
      loggedStatus: null
    }
  },
  methods: {
    changeStatus: function() {
      if(this.loggedStatus) {
        this.loggedStatus = false
      } else {
        this.loggedStatus = true
      }
    }
  },
  mounted: function() {
    let token = localStorage.getItem('token')
    if(token) {
      this.loggedStatus = true
    } else {
      this.loggedStatus = false
    }
    this.$store.dispatch("decode");
  },
    computed: mapState({
    error_status: state => state.error_status,
    error_msg: state => state.error_msg,
  }),

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
body {
  background-color: lightgoldenrodyellow
}
</style>
