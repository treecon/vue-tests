<template>
    <h3>{{name}}</h3>
    <div id="map">lalala</div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import L from 'leaflet';

export default {
    name: 'Map',
    props: {
        name: null
    },
    data() {
        return {
            map: null,
            points: null
        }
    },
    mounted() {
        this.emitter.on('addPoint', (data) => {
            L.circleMarker([data.lat, data.lon]).addTo(this.points).bindPopup(data.user).openPopup();
            this.map.fitBounds(this.points.getBounds());
        })

        this.map = L.map("map").setView([51.959, -8.623], 12);
        L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
            attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(this.map);
        this.points = L.featureGroup().addTo(this.map);
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    #map {
        height: 400px;
        width: 100%;
    }
</style>
