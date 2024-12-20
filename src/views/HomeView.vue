
<template>
  <div class="home">
    <header class="hero-section">
      <img alt="Vue logo" src="../assets/logo.png">
      <h1 class="display-4">Práctica 1</h1>
      <p class="lead">Práctica 1 del módulo 6 - Front End con VUE.js del Diplomado en FullStack Developer</p>
    </header>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  }
}
</script>
