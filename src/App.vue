<template>
  <NavBar />
  <router-view :key="$route.fullPath" v-if="isRouterAlive"/>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap/dist/js/bootstrap'
import NavBar from './components/NavBar'

export default {
  name: "App",
  components: {
    NavBar,
  },
  provide () {
    return {
      reload: this.reload
    }
  },
  data() {
    return {
      isRouterAlive: true
    }
  },
  methods: {
    reload () {
      this.isRouterAlive = false;
      this.$nextTick(function() {
        this.isRouterAlive = true;
      })
    }
  }
}
</script>

<style>
</style>
