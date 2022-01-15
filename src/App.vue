<template>
  <div id="wrapper">

    <router-link to="/">Djackets</router-link>
    <router-link to="/summer">Summer</router-link>
    <router-link to="/winter">Winter</router-link>
    
    <div class="buttons">
      <router-link to="/log-in">Log in</router-link>
      <router-link to="/cart">
        <span class="icon"><i class="fas fa-shopping-cart"></i></span>
        <span>Cart</span>
      </router-link>
    </div>

    <section class="section">
      <router-view />
    </section>

    <footer class="footer">
      <p class="">Copyright (c) 2022</p>
    </footer>
    
  </div>
</template>

<style lang="scss">
</style>
