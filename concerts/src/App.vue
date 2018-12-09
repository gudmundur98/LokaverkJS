<template>
  <div id="app">
    <b-container>
      <div>
        <b-navbar type="dark" variant="primary" toggleable class="mb-4">
          <b-navbar-toggle target="nav_dropdown_collapse"></b-navbar-toggle>
          <b-collapse is-nav id="nav_dropdown_collapse">
            <b-navbar-nav>
              <b-nav-item href="#">Concerts</b-nav-item>
            </b-navbar-nav>
          </b-collapse>
        </b-navbar>
      </div>

      <Conerts/>
    </b-container>
  </div>
</template>

<script>
import Conerts from './components/Concerts.vue'

export default {
  name: 'app',
  components: {
    Conerts
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
