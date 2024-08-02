<template>
    <section id="filter">
        <div class="d-flex justify-content-center">
            <select name="travel" id="travel" class="form-select">
                <option value="all">All</option>
            </select>
        </div>
    </section>

    <section id="travel-map" class="py-4">
        <div class="d-block map-container">
            <Map></Map>
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
            menu: [
                { name: 'All', value: 'All' },
                { name: 'Italy', value: 'Italy' },
                { name: 'Scotland', value: 'Scotland' },
                { name: 'Thailand', value: 'Thailand' },
            ],
        }
    }
}
</script>

<style lang="scss" scoped>
select {
    width: 100px;
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
