<template>
  <router-view />
</template>

<script>

export default {
  name: 'App',
}
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px; 
} */

html {
  background-color: black;
  -ms-overflow-style: none;
  /* IE and Edge */
  scrollbar-width: none;
  /* Firefox */
  scroll-behavior: smooth;
}

/* Hide scrollbar for Chrome, Safari and Opera */
html::-webkit-scrollbar {
  display: none;
}

#app {
  min-height: 100vh;
  display: grid;
  grid-template-rows: 1fr auto;
}

</style>
