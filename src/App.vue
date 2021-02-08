<template>
  <Navbar />

  <router-view />
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "App",
  components: { Navbar },
};
</script>

<style>
body {
  background-color: #f2f2f2;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  max-width: 37.5rem;
  margin: 0 auto;
  color: #555;
  padding-bottom: 1rem;
}

p {
  margin-top: 0;
}
</style>
