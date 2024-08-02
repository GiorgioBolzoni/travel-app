<template>
  <header class="d-flex align-items-center">
    <h1>
        <img src="/public/img/nomads-logo.png" alt="">
    </h1>
  </header>
</template>


<script>
export default {
    name: 'Header',
};
</script>


<style lang="scss" scoped>
header {
    height: 200px;
}
    h1{
        text-align: center;
        display: block;
        width: 100%;
        img{
            width: 25%;
            min-width: 200px;
        }
    }
</style>