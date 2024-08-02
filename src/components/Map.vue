<template>
    <div id="map" class="rounded-3"></div>
</template>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import { markers } from '../data/store.js';  

export default {
    name: 'Map',
    mounted() {
        this.initMap();
    },
    methods: {
        initMap() {
            // Inizializza la mappa centrata sull'Italia
            const map = L.map('map').setView([41.8719, 12.5674], 5);

            L.tileLayer(
                'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
                {
                    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
                }
            ).addTo(map);

            markers.forEach(marker => {
                L.marker(marker.position).addTo(map).bindPopup(`
                    <div class="popup-content">
                        <h3>${marker.title}</h3>
                        <div class="popup-image-container card">
                            <img src="${marker.image}" alt="${marker.title}" class="popup-image card-img"/>
                        </div>
                    </div>
                `);
            });
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
}


</style>
