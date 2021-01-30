<template>
  <div class="container-fluid">
    <Navbar name='My App' />
    <div class="row p-0 m-0 mt-0">
      <div class="col-md-9 p-0">
        <Map />
      </div>
      <div class="col-md-3">
        <GeometryForm @onAddPoint="onAddPoint" />
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import Map from './components/Map.vue'
import GeometryForm from './components/GeometryForm.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    Map,
    GeometryForm
  },
  methods: {
    onAddPoint(data) {
      console.log(data);
    }
  }
}
</script>

<style>
body {
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
