<template>
  <div class="navbar">
    <router-link :to="{ name: 'Home' }">My list</router-link>
    <router-link :to="{ name: 'AddProject' }">Add new</router-link>
  </div>
</template>

<script>
export default {
  name: "Navbar",
};
</script>

<style>
.navbar {
  text-align: center;
  margin: 30px auto;
}

.navbar a {
  display: inline-block;
  text-decoration: none;
  margin: 0 10px;
  color: #999;
  font-size: 18px;
}

.navbar a.router-link-active {
  border-bottom: 2px solid #25d483;
  padding-bottom: 4px;
}
</style>
