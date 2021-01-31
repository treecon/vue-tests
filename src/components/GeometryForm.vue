<template>
    <div class="form-group">
        <label for="inputCity">City</label>
        <input v-model="city" type="text" class="form-control" id="inputCity">
        <ul class="list-group">
            <li class="list-group-item" v-for="(city, index) in citiesSearchResults" v-bind:key="city in citiesSearchResults" @click="addToMap(index)">{{city.display_name}}</li>
        </ul>
    </div>
    <button class="btn btn-outline-danger float-right" v-if="showClearButton" @click="onClear()">Clear Points</button>
</template>

<script>
import axios from 'axios';

export default {
    name: 'GeometryForm',
    props: {
    },
    data() {
        return {
            showClearButton: false,
            city: null,
            debounceTimeoutRef: null,
            debounceDelay: 400,
            citiesSearchResults: null
        }
    },
    watch: {
        city: function(searchValue) {
            //debounce for city search input
            if (this.debounceTimeoutRef) {
                clearTimeout(this.debounceTimeoutRef);
            }
            this.debounceTimeoutRef = setTimeout(() => {
                this.getCitiesFromGeocodingAPI(searchValue);
            }, this.debounceDelay);
        }
    },
    mounted() {
    },
    methods: {
        getCitiesFromGeocodingAPI(val) {
            if (!val) {
                return;
            }
            let queryString = `https://nominatim.openstreetmap.org/search?q=${val.split(' ').join('+')}&format=json&limit=5`;
            
            axios.get(queryString)
                .then(response => response.data)
                .then(response => {
                    console.log(response);
                    this.citiesSearchResults = response;
                });
        },

        addToMap(index) {
            let data = {
                city: this.citiesSearchResults[index].display_name.split(',')[0],
                lat: this.citiesSearchResults[index].lat,
                lon: this.citiesSearchResults[index].lon
            };
            this.citiesSearchResults = null;
            this.showClearButton = true;

            this.emitter.emit('addPoint', data);
            this.city = null;
        },

        onClear() {
            this.emitter.emit('clearPoints');
            this.showClearButton = false;
        }
    }
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
