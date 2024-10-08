# Travel App

## NOMADS: il diario di viaggio delle tue vacanze estive

### OBIETTIVO

```
Realizzare una Web App che consenta di annotare le diverse esperienze di viaggio, anche mentre questo è in corso.
```

---

### FUNZIONALITA'

L'applicazione permette di:

- Visualizzare i propri viaggi, filtrabili anche per anno e Paese, dalla mappa
- Esplorare la mappa
- Visualizzare i dettagli di ogni viaggio
- Aggiungere diversi dettagli al singolo viaggio, come:
  - data di inizio e data di fine del viaggio
  - aggiungere nuovi giorni al viaggio
  - annotare giorno per giorno le diverse esperienze
  - inserire foto per ogni giorno del viaggio
  - aggiungere, e visualizzare, un voto da 1 a 5 relativo al viaggio in generale, ma anche ad alcuni aspetti come: cibo, paesaggio, attività, svago, relax, etc.

I dati inseriti verranno salvati in locale per poterli visualizzare in ogni momento.

**_Alcuni dettagli:_**

- Selezionando uno specifico Paese dall'elenco, la mappa effettuerà uno zoom, precedentemente impostato con i limiti geografici, sul quel Paese
- Zoom in e zoom out hanno un limite impostato manualmente
- Possibilità di eliminare dalla lista un giorno di viaggio

---

### FUTURE INTEGRAZIONI

Sono previste nuove funzionalità, l'integrazione delle stesse avverrà con seguente ordine

1. Possibilità di aggiungere nuovi viaggi da App
2. Upgrade del layout
3. TBD ...

---

### TECNOLOGIE UTILIZZATE

```
Vue.js
JavaScript
Scss
```

---

### COME VENGONO SALVATI I DATI?

```
Local Storage
```

---

### INTEGRAZIONE DELLA MAPPA

La MAPPA è stata integrata tramite il servizio offerto da **[Leaflet](https://leafletjs.com/)**.

Inoltre sono stati personalizzati i marker, sia nella forma che nel colore, per potergli attribuire diversi significati (vedi legenda).

I marker sono stati posizionati sulla mappa utilizzando le coordinate delle varie città, ovvero altitudine e longitudine.

---

### SERVIZIO DI HOSTING SCELTO

**[Netlify](https://www.netlify.com/)**

---

### APP ONLINE

**[NOMADS](https://nomads-travel-app.netlify.app/)**
