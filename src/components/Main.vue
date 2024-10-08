<template>
  <section id="filter">
    <div class="d-flex justify-content-center align-items-center">
      <span>Where:</span>
      <select name="travel" id="travel" class="form-select mx-3" v-model="selectedCountry" @change="applyFilters">
        <option value="Italia">Italia</option>
        <option value="All">Tutti i paesi</option>
        <option v-for="country in sortedCountries" :key="country" :value="country">
          {{ country }}
        </option>
      </select>
    </div>
    <div class="d-flex justify-content-center align-items-center mt-3">
      <span>When:</span>
      <select name="year" id="year" class="form-select mx-3" v-model="selectedYear" @change="applyFilters">
        <option value="">Tutti gli anni</option>
        <option value="<1"> < 1 anno</option>
        <option value="<5"> < 5 anni fa</option>
        <option value="<10"> < 10 anni fa</option>
        <option value=">10"> > 10 anni fa</option>
      </select>
    </div>
  </section>

  <section id="travel-map" class="py-4">
    <div class="d-block map-container">
      <Map ref="mapComponent" :markers="filteredMarkers" :selectedCountry="selectedCountry" @openModal="openModal" />
    </div>
  </section>

  <Modal v-if="isModalVisible" :show="isModalVisible" :marker="selectedMarker" @close="closeModal" />
</template>

<script>
import Map from './Map.vue';
import Modal from './Modal.vue';  
import { markers } from '../data/store.js';

export default {
  name: 'Main',
  components: {
    Map,
    Modal 
  },
  data() {
    return {
      selectedCountry: 'Spagna',
      selectedYear: '',
      markers,
      filteredMarkers: markers,
      countries: [
        'Italia',
        'Inghilterra',
        'Scozia',
        'Thailandia',
        'Canada',
        'Danimarca',
        'USA',
        'Brasile',
        'Cuba',
        'Messico',
        'Spagna',
        'Francia',
        'Grecia',
        'Turchia',
        'Ungheria',
        'Tunisia',
        'Marocco',
        'Maldive',
        'Egitto'
      ],
      isModalVisible: false,  
      selectedMarker: null  // Marker selezionato per la modale
    };
  },
  computed: {
    sortedCountries() {
      return this.countries.sort();
    }
  },
  methods: {
    applyFilters() {
      // Prima filtro i marker
      this.filterMarkers();
      // Poi aggiorno la mappa con i marker filtrati
      this.updateMap();
    },
    filterMarkers() {
      const currentYear = new Date().getFullYear();
      let filtered = this.markers;

      if (this.selectedCountry !== 'All') {
        filtered = filtered.filter(marker => marker.country === this.selectedCountry);
      }

      if (this.selectedYear === '<1') {
        filtered = filtered.filter(marker => marker.year && (currentYear - marker.year) < 1);
      } else if (this.selectedYear === '<5') {
        filtered = filtered.filter(marker => marker.year && (currentYear - marker.year) < 5);
      } else if (this.selectedYear === '<10') {
        filtered = filtered.filter(marker => marker.year && (currentYear - marker.year) < 10);
      } else if (this.selectedYear === '>10') {
        filtered = filtered.filter(marker => marker.year && (currentYear - marker.year) > 10);
      }

      // Aggiungo sempre Milano ai marker filtrati
      const milanoMarker = this.markers.find(marker => marker.title === 'Milano, Italia');
      if (milanoMarker) {
        filtered = [...filtered, milanoMarker];
      }

      this.filteredMarkers = filtered;
    },
    updateMap() {
      const mapComponent = this.$refs.mapComponent;
      if (mapComponent) {
        mapComponent.fitBoundsForCountry(this.selectedCountry);
      }
    },
    openModal(marker) {  
      this.selectedMarker = marker;
      this.isModalVisible = true;
    },
    closeModal() {  
      this.isModalVisible = false;
      this.selectedMarker = null;
    }
  },
  mounted() {
    this.applyFilters(); // Applica i filtri inizialmente per impostare i marker e la mappa
  }
};
</script>

<style lang="scss" scoped>
span {
  color: rgb(255, 255, 255);
  font-weight: bold;
}
select {
  width: 150px;
  &:hover {
    cursor: pointer;
  }
  option {
    &:hover {
      cursor: pointer !important;
    }
  }
}
#travel-map {
  height: 100%;
}
.map-container {
  height: 70vh;
  width: 100%;
}

@media (max-width: 768px) {
  .map-container {
    height: 60vh;
  }
}

@media (max-width: 480px) {
  .map-container {
    height: 50vh;
  }
}
</style>
