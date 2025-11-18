<template>
  <div id="app">
    <NavBar />
    <router-view :key="$route.fullPath" />
  </div>
</template>


<script>
import NavBar from './components/NavBar.vue';

export default {
  name: 'App',
  components: {
    NavBar: NavBar
  }
}
</script>

<style></style>
