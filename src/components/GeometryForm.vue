<template>
    <div class="form-group">
        <label for="inputCity">city</label>
        <input v-debounce="debounceDelay" v-model.lazy="city" type="text" class="form-control" id="inputCity">
        <ul class="list-group">
            <li class="list-group-item" v-for="(city, index) in citiesResults" v-bind:key="city in citiesResults" @click="addToMap(index)">{{city.display_name}}</li>
        </ul>
    </div>
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
</template>

<script>
import debounce from 'v-debounce';
import axios from 'axios';

export default {
    name: 'GeometryForm',
    props: {
    },
    data() {
        return {
            city: null,
            user: null,
            lat: null,
            lon: null,
            debounceDelay: 400,
            citiesResults: null
        }
    },
    watch: {
        city: function(value) {
            let queryString = `https://nominatim.openstreetmap.org/search?q=${value.split(' ').join('+')}&format=json&limit=5`;
            console.log(queryString);
            //check axios built-in debounce method
            axios.get(queryString)
                .then(response => response.data)
                .then(response => {
                    console.log(response);
                    this.citiesResults = response;
                });
        }
    },
    mounted() {
    },
    methods: {
        addToMap(index) {
            console.log(index);
            
            let data = {
                city: this.citiesResults[index].display_name.split(',')[0],
                lat: this.citiesResults[index].lat,
                lon: this.citiesResults[index].lon
            }
            this.citiesResults = null;

            this.emitter.emit('addPoint', data);
        }
    },
    directives: {debounce}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .list-group {
        position: absolute;
    }

    .list-group-item {
        background: #ddd;
    }

    .list-group-item:hover {
        background: dodgerblue;
        cursor: pointer;
    }
</style>
