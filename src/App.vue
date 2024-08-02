<template>

<div class="container">
  <Header></Header>

  <Main></Main>
</div>

</template>


<script>
import Header from './components/Header.vue';
import Main from './components/Main.vue';

export default {
    components:{
      Header,
      Main,
    },
    data(){
      return{
      }
    },
    methods:{
    },
    mounted() {
      
    }
    }
</script>


<style lang="scss" scoped>

</style>