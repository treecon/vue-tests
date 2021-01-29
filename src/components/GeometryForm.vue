<template>
    <div class="formContainer">
        <div class="form-group">
            <label for="inputName">name {{user}}</label>
            <input v-model="user" type="text" class="form-control" id="inputName">
        </div>
        <div class="form-group">
            <label for="inputLat">latitude</label>
            <input v-model="lat" type="text" class="form-control" id="inputLat">
        </div>
        <div class="form-group">
            <label for="inputLon">longitude</label>
            <input v-model="lon" type="text" class="form-control" id="inputLon">
        </div>
        <button @click="addToMap()" type="submit" class="btn btn-primary">Submit</button>
    </div>
</template>

<script>
export default {
    name: 'GeometryForm',
    props: {
    },
    data() {
        return {
            user: null,
            lat: null,
            lon: null
        }
    },
    mounted() {
    },
    methods: {
        addToMap() {
            let data = {
                user: this.user,
                lat: this.lat,
                lon: this.lon
            }
            this.emitter.emit('addPoint', data);
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .formContainer {
        max-width: 400px;
    }
</style>
