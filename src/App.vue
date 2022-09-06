<template>
  <Navbar />
  <router-view />
  <Footer />
</template>

<script>
import Navbar from './components/Navbar.vue';
import Footer from './components/Footer.vue';

export default {
  components:{
    Navbar,
    Footer
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=DynaPuff&display=swap');
    * {
        font-family: 'DynaPuff', cursive;
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }



</style>
