<template>
    <div>
      <h3>Diario di viaggio</h3>
      <div v-for="(entry, index) in entries" :key="index" class="entry">
        <h4>Giorno {{ index + 1 }}</h4>
        <div class="entry-content">
          <div class="photo">
            <img :src="entry.photo" alt="Foto del giorno">
            <input type="file" @change="updatePhoto($event, index)" />
          </div>
          <textarea v-model="entry.notes" placeholder="Note della giornata"></textarea>
        </div>
      </div>
      <button @click="addEntry">Aggiungi giorno</button>
      <button @click="save">Salva</button>
    </div>
  </template>
  
  <script>
  export default {
    props: ['marker'],
    data() {
      return {
        entries: []
      }
    },
    mounted() {
      this.loadEntries();
    },
    methods: {
      addEntry() {
        this.entries.push({ photo: '', notes: '' });
      },
      updatePhoto(event, index) {
        const file = event.target.files[0];
        const reader = new FileReader();
        reader.onload = (e) => {
          this.entries[index].photo = e.target.result;
        }
        reader.readAsDataURL(file);
      },
      save() {
        localStorage.setItem(`entries_${this.marker.id}`, JSON.stringify(this.entries));
        alert('Salvato con successo!');
      },
      loadEntries() {
        const storedEntries = localStorage.getItem(`entries_${this.marker.id}`);
        if (storedEntries) {
          this.entries = JSON.parse(storedEntries);
        }
      }
    }
  }
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
  </style>
  