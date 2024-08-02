<template>
    <div id="map" class="rounded-3"></div>
</template>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import { markers, countryBounds } from '../data/store.js';

export default {
    name: 'Map',
    props: ['country'],
    mounted() {
        this.initMap();
    },
    watch: {
        country: 'updateMap'
    },
    methods: {
        initMap() {
            this.map = L.map('map', {
                center: [41.8719, 12.5674],
                zoom: 5,
                maxBounds: [[-90, -180], [90, 180]], // Limita la visualizzazione della mappa
                minZoom: 2  
            }); // Italia come predefinito

            L.tileLayer(
                'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
                {
                    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
                }
            ).addTo(this.map);

            this.addMarkers();
        },
        addMarkers() {
            markers.forEach(marker => {
                L.marker(marker.position).addTo(this.map).bindPopup(`
                    <div class="popup-content card border-0">
                        <h3>${marker.title}</h3>
                        <img src="${marker.image}" alt="${marker.title}" class="popup-image"/>
                    </div>
                `);
            });
        },
        updateMap() {
            if (this.country && countryBounds[this.country]) {
                const bounds = countryBounds[this.country];
                this.map.fitBounds(bounds);
            }
        },
        fitBoundsForCountry(country) {
            const bounds = countryBounds[country];
            if (bounds) {
                this.map.fitBounds(bounds);
            }
        }
    }
}
</script>

<style scoped>
#map {
    height: 100%;
    width: 100%;
}

.popup-content {
    text-align: center;
    height: 100px;
    overflow: hidden;
}

.popup-image {
    width: 100%;
    max-height: 70px;
    border-radius: 5px;
}
</style>
