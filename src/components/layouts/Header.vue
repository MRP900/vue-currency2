<template>
  <header class="header">
    <h3 id="title">Currency Converter</h3>
    <div id="nav">
      <router-link to="/">Home</router-link> | 
      <router-link to="/about">About</router-link>
    </div>
  </header>
</template>

<script>
export default {
  name: "Header"
}
</script>

<style scoped>
  .header {
    background: rgb(105, 3, 3);
    color: #fff;
    text-align: center;
    padding: 5px;
    height: 5.5em;
  }
  a {
    color: #fff;
    padding-right: 5px;
    padding-left: 5px;
    text-decoration: none;
  }

  #title {
    margin: .25em 0;
  }

  /* #nav {

  } */
  /* .router-link-exact-active {
      color: orange;
      text-decoration: underline;
    } */
</style>