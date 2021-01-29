<template>
  <img alt="Vue logo" src="./assets/logo.png">

  <div class="container-fluid">
    <div class="row">
      <div class="col-md-6">
        <Map name='My Map' />
      </div>
      <div class="col-md-6">
        <GeometryForm @onAddPoint="onAddPoint" />
      </div>
    </div>
  </div>
</template>

<script>
import Map from './components/Map.vue'
import GeometryForm from './components/GeometryForm.vue'

export default {
  name: 'App',
  components: {
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
