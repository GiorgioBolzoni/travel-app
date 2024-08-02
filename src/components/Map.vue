<template>
    <div id="map" class="rounded-3">
      <div class="legend">
        <div><span class="legend-color green my-1"></span> Mi trovo qui</div>
        <div><span class="legend-color red my-1"></span> Vivo qui</div>
      </div>
    </div>
  </template>
  
  <script>
  import L from 'leaflet';
  import 'leaflet/dist/leaflet.css';
  import { countryBounds } from '../data/store.js';
  
  // Funzioni per creare icone di marker colorati
  const createIcon = (color) => {
    return L.divIcon({
      className: 'custom-icon',
      html: `<div style="background-color: ${color}; border-radius: 50%; width: 20px; height: 20px; border: 2px solid white;"></div>`,
      iconSize: [20, 20]
    });
  };
  
  export default {
    name: 'Map',
    props: ['markers', 'selectedCountry'],
    mounted() {
      this.initMap();
    },
    watch: {
      markers: 'updateMarkers',
      selectedCountry: 'updateMap'
    },
    methods: {
      initMap() {
        this.map = L.map('map', {
          center: [41.8719, 12.5674],
          zoom: 5,
          maxBounds: [[-90, -180], [90, 180]], // Limita la visualizzazione della mappa
          minZoom: 2  
        });
  
        L.tileLayer(
          'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
          {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
          }
        ).addTo(this.map);
  
        this.addMarkers();
      },
      addMarkers() {
        this.markers.forEach(marker => {
          let iconColor = 'blue'; // Default color for other markers
  
          if (marker.title === 'Cala d\'Or, Palma di Maiorca, Spagna') {
            iconColor = 'green'; // Special marker for 'Sono qui'
          } else if (marker.title === 'Milano, Italia') {
            iconColor = 'red'; // Special marker for Milano
          }
  
          L.marker(marker.position, { icon: createIcon(iconColor) })
            .addTo(this.map)
            .bindPopup(`
              <div class="popup-content card border-0">
                <h3>${marker.title}</h3>
                <img src="${marker.image}" alt="${marker.title}" class="popup-image"/>
                <p>Anno di visita: ${marker.year}</p>
              </div>
            `);
        });
      },
      updateMarkers() {
        this.map.eachLayer(layer => {
          if (layer instanceof L.Marker) {
            this.map.removeLayer(layer);
          }
        });
        this.addMarkers();
      },
      updateMap() {
        if (this.selectedCountry === 'All') {
          this.map.setView([0, 0], 2); // Zoom out per vedere l'intero mondo
        } else if (this.selectedCountry && countryBounds[this.selectedCountry]) {
          const bounds = countryBounds[this.selectedCountry];
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
  };
  </script>
  
  <style scoped>
  #map {
    height: 100%;
    width: 100%;
    position: relative; 
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
  
  /* Custom styles for colored markers */
  .custom-icon {
    border-radius: 50%;
    width: 20px;
    height: 20px;
  }
  
  /* Styles for legend */
  .legend {
    position: absolute;
    top: 10px;
    right: 10px;
    background: white;
    border: 2px solid #ccc;
    border-radius: 5px;
    padding: 10px;
    font-size: 12px;
    z-index: 1000; 
    box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.5);
  }
  
  .legend-color {
    display: inline-block;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    margin-right: 5px;
    vertical-align: middle;
  }
  
  .green {
    background-color: green;
  }
  
  .red {
    background-color: red;
  }
  </style>
  