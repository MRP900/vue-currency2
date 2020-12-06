<template>
  <header class="header">
    <h4 id="title">Currency Converter</h4>
    <div id="nav">
      <router-link to="/">Home</router-link><span id="pipe">|</span> 
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
    background: rgb(134, 102, 102);
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
    margin: .15em 0;
  }

  @media only screen and (max-width: 600px) {
    
    
    #title {
      margin: .1em auto;
    }
}

@media only screen and (max-width: 400px) {
#nav {
      display: flex;
      flex-direction: column;
    }
    #pipe{
      display: none;
    }
    .header {
      height: 8em;
    }
}

  /* .router-link {
      margin: .25em auto;
    } */
</style>