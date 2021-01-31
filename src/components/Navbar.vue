<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <img alt="Vue logo" src="../assets/logo.png" height="70">
        <a class="navbar-brand" href="#">{{name}}</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav ml-auto mr-0">
                <li class="nav-item">
                    <span v-if="points.length == 0">Add at least 2 cities to measure distance</span>
                    <span v-else-if="points.length == 1">Add at least one more city to measure distance</span>
                    <span v-else>Distance: {{distance.toFixed(2)}} km</span>
                </li>
            </ul>
        </div>
    </nav>
</template>

<script>
import turfDistance from '@turf/distance';
import * as turfHelpers from '@turf/helpers';

export default {
    name: 'Navbar',
    props: ['name'],
    data() {
        return {
            points: [],
            distance: null
        }
    },
    mounted() {
        this.emitter.on('addPoint', (point) => {
            this.points.push(point);
            this.distance = this.computeDistance(this.points);
        });
        this.emitter.on('clearPoints', () => {
            this.clearDistance();
        });
    },
    methods: {
        computeDistance(points) {
            if (points.length < 2) {
                return 0;
            }
            let totalDistance = 0;
            for (let i=1; i<points.length; i++) {
                let pointA = turfHelpers.point([points[i-1].lat, points[i-1].lon]);
                let pointB = turfHelpers.point([points[i].lat, points[i].lon]);
                let distance = turfDistance(pointA, pointB, {units: 'kilometers'});
                totalDistance += distance;
            }
            return totalDistance;
        },
        clearDistance() {
            this.points = [];
            this.distance = null;
        }
    }
}
</script>
<style>
</style>