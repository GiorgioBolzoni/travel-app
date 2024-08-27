<template>
    <div>
      <h3>Diario di viaggio</h3>
      <div v-for="(entry, index) in entries" :key="index" class="entry">
        <h4>Giorno {{ index + 1 }}</h4>
        <div class="entry-content">
          <div class="photo my-3">
            <img :src="entry.photo" alt="Foto del giorno">
            <input type="file" @change="updatePhoto($event, index)" class="mx-2" />
          </div>
          <textarea v-model="entry.notes" @input="save" placeholder="Note della giornata"></textarea>
        </div>
      </div>
      <div>
        <button @click="addEntry" class="mx-2 btn btn-primary">Aggiungi giorno</button>
        <button @click="save" class="mx-2 btn btn-success">Salva</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['marker'],
    data() {
      return {
        entries: []
      };
    },
    mounted() {
      this.loadEntries();
    },
    methods: {
      addEntry() {
        this.entries.push({ photo: '', notes: '' });
        this.save(); // Salva subito dopo l'aggiunta di un nuovo giorno
      },
      updatePhoto(event, index) {
        const file = event.target.files[0];
        const reader = new FileReader();
        reader.onload = (e) => {
          this.entries[index].photo = e.target.result;
          this.save(); // Salva subito dopo l'aggiornamento della foto
        };
        reader.readAsDataURL(file);
      },
      save() {
        localStorage.setItem(`entries_${this.marker.id}`, JSON.stringify(this.entries));
        console.log('Dati salvati con successo!');
      },
      loadEntries() {
        const storedEntries = localStorage.getItem(`entries_${this.marker.id}`);
        if (storedEntries) {
          this.entries = JSON.parse(storedEntries);
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
  </style>
  