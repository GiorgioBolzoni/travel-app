<template>
    <div class="modal" v-if="show">
      <div class="modal-content">
        <span class="close" @click="close">&times;</span>
        <h2 class="text-center mark-title">{{ marker.title }}</h2>
        <Travelogue :marker="marker" />
      </div>
    </div>
  </template>
  
  <script>
  import Travelogue from './Travelogue.vue';
  
  export default {
    props: ['show', 'marker'],
    components: {
      Travelogue
    },
    methods: {
      close() {
        this.$emit('close');
      }
    }
  };
  </script>
  
  <style scoped>
  .modal {
    display: block;
    position: fixed;
    z-index: 1000;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0,0,0,0.4);
  }
  .mark-title{
    font-weight: bolder;
    color: #db4205c2;
    font-size: 2.5em;
  }
  .modal-content {
    background-color: #fefefe;
    margin: 15% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
  }
  
  .close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
  }
  
  .close:hover,
  .close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
  }
  </style>