<template>
  <div>
    <div class="date-picker-container my-2">
      <label for="start-date">Da:</label>
      <datepicker v-model="startDate" placeholder="Seleziona data inizio" />
      <label for="end-date">A:</label>
      <datepicker v-model="endDate" placeholder="Seleziona data fine" />
    </div>
    
    <div class="my-3">
      <label for="rating">Valutazione del viaggio:</label>
      <vue3-star-ratings 
        v-model="rating"
        :starSize="32"
        starColor="#ff9800"
        inactiveColor="#333333"
        :numberOfStars="5"
        :disableClick="false" 
        @rating-selected="save" />
    </div>

    <h3>Diario di viaggio</h3>
    
    <div v-for="(entry, index) in entries" :key="index" class="entry">
      <h4>
        Giorno {{ index + 1 }}
        <button @click="deleteDay(index)" class="btn btn-danger" style="width: min-content; height: min-content; align-self: center;">
          Delete
        </button>
      </h4>
      <div class="entry-content row">
        <div class="photo my-3 col-12 col-lg-5">
          <img :src="entry.photo" alt="Foto del giorno">
          <input type="file" @change="updatePhoto($event, index)" class="m-2 text-wrap" />
        </div>
        <textarea v-model="entry.notes" @input="save" placeholder="Note della giornata" class="rounded-2 col-12 col-lg-5"></textarea>
      </div>
    </div>
    
    <div>
      <button @click="addEntry" class="mx-2 btn btn-primary">Aggiungi giorno</button>
      <button @click="save" class="mx-2 btn btn-success">Salva</button>
    </div>
  </div>
</template>

<script>
import Datepicker from 'vue3-datepicker';
import Vue3StarRatings from 'vue3-star-ratings';

export default {
  components: {
    Datepicker,
    Vue3StarRatings
  },
  props: ['marker'],
  data() {
    return {
      entries: [], 
      startDate: null, 
      endDate: null,
      rating: 0 
    };
  },
  mounted() {
    this.loadEntries();
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
      this.entries.splice(index, 1);
      this.save();
    },
    save() {
      const dataToSave = {
        entries: this.entries,
        startDate: this.startDate,
        endDate: this.endDate,
        rating: this.rating 
      };
      localStorage.setItem(`entries_${this.marker.id}`, JSON.stringify(dataToSave));
      console.log('Dati salvati con successo!');
    },
    loadEntries() {
      const storedData = localStorage.getItem(`entries_${this.marker.id}`);
      if (storedData) {
        const parsedData = JSON.parse(storedData);
        this.entries = parsedData.entries || [];
        this.startDate = parsedData.startDate ? new Date(parsedData.startDate) : null;
        this.endDate = parsedData.endDate ? new Date(parsedData.endDate) : null;
        this.rating = parsedData.rating || 0; 
      }
    }
  }
};
</script>

<style scoped>
.entry {
  margin-top: 20px;
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
</style>
