<template>
    <div id="map"></div>
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
            points: [],
            pointsFeatGroup: null,
            linesFeatGroup: null
        }
    },
    mounted() {
        this.emitter.on('addPoint', (point) => {
            this.addPoint(point);
        });
        this.emitter.on('clearPoints', () => {
            this.clearPoints();
        });

        this.map = L.map("map").setView([51.959, -8.623], 12);
        L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
            attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(this.map);
        this.pointsFeatGroup = L.featureGroup().addTo(this.map);
        this.linesFeatGroup = L.featureGroup().addTo(this.map);
    },
    methods: {
        addPoint(point) {
            this.points.push(point);
            L.circleMarker([point.lat, point.lon]).addTo(this.pointsFeatGroup).bindPopup(point.city).openPopup();
            
            if (this.points.length >= 2) {
                let pointA = [this.points[this.points.length - 2].lat, this.points[this.points.length - 2].lon];
                let pointB = [point.lat, point.lon];
                L.polyline([pointA, pointB]).addTo(this.linesFeatGroup);
            }

            this.map.fitBounds(this.pointsFeatGroup.getBounds(), {maxZoom: 10});
        },
        clearPoints() {
            this.points = [];
            this.pointsFeatGroup.clearLayers();
            this.linesFeatGroup.clearLayers();
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    #map {
        height: 100%;
        width: 100%;
    }
</style>
