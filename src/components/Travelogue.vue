<template>
  <div>
    <!-- Sezione Datepicker -->
    <div class="date-picker-container my-2 row justify-content-center">
      <div class="d-flex flex-column align-items-center gap-2 col-12 col-md-6 calendar">
        <label for="date" class=" fw-bold fs-5">Quando</label>
        <VueDatePicker v-model="dateRange" range :multi-calendars="true" placeholder="Seleziona date" />
      </div>
    </div>
    
    <!-- Sezione per le valutazioni -->
    <div class="row justify-content-center fw-bold">
      <div class="starRating col-12 col-sm-6 col-md-4 col-lg-2">
        <label for="travelRating">Overall:</label>
        <vue3-star-ratings 
          v-model="travelRating"
          :starSize="16"
          starColor="#ff9800"
          inactiveColor="#333333"
          :numberOfStars="5"
          :disableClick="false" 
          @rating-selected="save" 
        />
      </div>
      <div class="starRating col-12 col-sm-6 col-md-4 col-lg-2">
        <label for="foodRating">Cibo:</label>
        <vue3-star-ratings 
          v-model="foodRating"
          :starSize="16"
          starColor="#ff9800"
          inactiveColor="#333333"
          :numberOfStars="5"
          :disableClick="false" 
          @rating-selected="save" 
        />
      </div>
      <div class="starRating col-12 col-sm-6 col-md-4 col-lg-2">
        <label for="sceneryRating">Paesaggio:</label>
        <vue3-star-ratings 
          v-model="sceneryRating"
          :starSize="16"
          starColor="#ff9800"
          inactiveColor="#333333"
          :numberOfStars="5"
          :disableClick="false" 
        />
      </div>
      <div class="starRating col-12 col-sm-6 col-md-4 col-lg-2">
        <label for="activitiesRating">Attività:</label>
        <vue3-star-ratings 
          v-model="activitiesRating"
          :starSize="16"
          starColor="#ff9800"
          inactiveColor="#333333"
          :numberOfStars="5"
          :disableClick="false" 
        />
      </div>
      <div class="starRating col-12 col-sm-6 col-md-4 col-lg-2">
        <label for="priceRating">Prezzi:</label>
        <vue3-star-ratings 
          v-model="priceRating"
          :starSize="16"
          starColor="#ff9800"
          inactiveColor="#333333"
          :numberOfStars="5"
          :disableClick="false" 
        />
      </div>
    </div>

    <!-- Sezione note di viaggio -->

    <div class="my-2 blog p-3 rounded-3">
      <h3 class="my-2 text-center fw-bold">Diario di viaggio</h3>
    
        <div v-for="(entry, index) in entries" :key="index" class="entry d-flex flex-column my-2 p-3 rounded-3">
          <h4>
            Giorno {{ index + 1 }}
            <button @click="deleteDay(index)" class="btn btn-danger" style="width: min-content; height: min-content; align-self: center;">
              Delete
            </button>
          </h4>
          <div class="entry-content row p-2">
            <div class="photo my-3 col-12 col-lg-5">
              <img :src="entry.photo" alt="Foto del giorno">
              <input type="file" @change="updatePhoto($event, index)" class="m-2 text-wrap" />
            </div>
            <textarea v-model="entry.notes" @input="save" placeholder="Note della giornata" class="rounded-2 col-12 col-lg-5"></textarea>
          </div>
        </div>
        
        <div class="text-center mt-4 ">
          <button @click="addEntry" class="m-2 btn btn-primary">Aggiungi giorno</button>
          <button @click="save" class="m-2 btn btn-success">Salva</button>
        </div>
      </div>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
import Vue3StarRatings from 'vue3-star-ratings';

export default {
  components: {
    VueDatePicker,
    Vue3StarRatings,
  },
  props: ['marker'], // Usato per identificare univocamente il viaggio
  data() {
    return {
      dateRange: [],          // Per memorizzare l'intervallo di date selezionato
      travelRating: 0,        
      foodRating: 0,          
      sceneryRating: 0,       
      priceRating: 0,         
      activitiesRating: 0,    
      entries: [],            // Per memorizzare le voci del diario
    };
  },
  methods: {
    addEntry() {
      this.entries.push({ photo: '', notes: '' });
      this.save();
    },
    updatePhoto(event, index) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        this.entries[index].photo = e.target.result;
        this.save();
      };
      reader.readAsDataURL(file);
    },
    deleteDay(index) {
      if (index === this.entries.length - 1) {
        this.entries.splice(index, 1);
      } else {
        this.entries[index].photo = '';
        this.entries[index].notes = '';
      }
      this.save();
    },
    save() {
      // Usa l'id del viaggio come chiave per salvare i dati
      const dataToSave = {
        entries: this.entries,
        startDate: this.dateRange[0],
        endDate: this.dateRange[1],
        travelRating: this.travelRating,
        foodRating: this.foodRating,
        sceneryRating: this.sceneryRating,
        priceRating: this.priceRating,
        activitiesRating: this.activitiesRating,
      };
      localStorage.setItem(`travel_data_${this.marker.id}`, JSON.stringify(dataToSave));
      console.log('Dati salvati con successo per il viaggio ID:', this.marker.id);
    },
    loadEntries() {
      // Carica i dati in base all'id del viaggio
      const storedData = localStorage.getItem(`travel_data_${this.marker.id}`);
      if (storedData) {
        const parsedData = JSON.parse(storedData);
        this.entries = parsedData.entries || [];
        this.dateRange = [new Date(parsedData.startDate), new Date(parsedData.endDate)];
        this.travelRating = parsedData.travelRating || 0;
        this.foodRating = parsedData.foodRating || 0;
        this.sceneryRating = parsedData.sceneryRating || 0;
        this.priceRating = parsedData.priceRating || 0;
        this.activitiesRating = parsedData.activitiesRating || 0;
      }
    }
  },
  mounted() {
    this.loadEntries();
  }
};
</script>




<style scoped>
.calendar{
  max-width: 370px;
}
.blog {
  background-color: #db4205c2;
}
.btn{
  box-shadow: 3px 3px 5px #000000;
}
.entry{
  background-color: #212529;
  color: white;
}
.entry-content {
  display: flex;
  gap: 20px;
}

.photo img {
  max-width: 150px;
  max-height: 150px;
}

textarea {
  flex-grow: 1;
}

.date-picker-container {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
}

.starRating{
  margin-top: 10px;
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
