<template>
  <div class="container-fluid p-0 d-flex flex-column" style="height: 100vh">
    <Navbar name="My App" />
    <div class="row p-0 m-0" style="height: 100%; flex-grow: 1;">
      <div class="col-md-9 p-0">
        <Map />
      </div>
      <div class="col-md-3">
        <Form />
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import Map from './components/Map.vue'
import Form from './components/Form.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    Map,
    Form
  },
  methods: {
    onAddPoint(data) {
      console.log(data);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
