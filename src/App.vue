<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Main</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view />
  </div>
</template>

<style>
body {
  background-color: #1e1e1e !important;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

#nav {
  text-align: center;
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #4091e2;
  transition: color 0.4s ease-out;
}

#nav a:hover {
  text-decoration: none;
  color: white;
  transition: color 0.4s ease-in;
}
</style>
