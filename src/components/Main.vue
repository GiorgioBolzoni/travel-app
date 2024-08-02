<template>
    <section id="filter">
        <div class="d-flex justify-content-center">
            <select name="travel" id="travel" class="form-select" v-model="selectedCountry" @change="updateMap">
                <option value="Italia">Italia</option>
                <option v-for="country in sortedCountries" :key="country" :value="country">
                    {{ country }}
                </option>
            </select>
        </div>
    </section>

    <section id="travel-map" class="py-4">
        <div class="d-block map-container">
            <Map ref="mapComponent"></Map>
        </div>
    </section>
</template>

<script>
import Map from './Map.vue'

export default {
    name: 'Main',
    components: {
        Map,
    },
    data() {
        return {
            selectedCountry: 'Italia',
            countries: [
                'Italia',
                'Inghilterra',
                'Scozia',
                'Thailandia',
                'Canada',
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
                'Egitto'
            ]
        }
    },
    computed: {
        sortedCountries() {
            return this.countries.sort();
        }
    },
    methods: {
        updateMap() {
            const mapComponent = this.$refs.mapComponent;
            if (mapComponent) {
                mapComponent.fitBoundsForCountry(this.selectedCountry);
            }
        }
    },
    mounted() {
        this.updateMap(); 
    }
}
</script>

<style lang="scss" scoped>
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
