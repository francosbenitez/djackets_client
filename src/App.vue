<template>
  <div id="wrapper">

    <router-link to="/">Djackets</router-link>
    <router-link to="/summer">Summer</router-link>
    <router-link to="/winter">Winter</router-link>
    
    <div class="buttons">
      <router-link to="/log-in">Log in</router-link>
      <router-link to="/cart">
        <span class="icon"><i class="fas fa-shopping-cart"></i></span>
        <span>
          Cart
          ({{ cartTotalLength }})
        </span>
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

<script> 
export default {
  data() {
    return {
      cart: {
        items: []
      }
    }
  },
  // Before this component is created, we initialize the store
  // 'commit' is used to call the actions inside the mutations
  beforeCreate() {
    this.$store.commit('initializeStore')
  },
  mounted() {
    this.cart = this.$store.state.cart
  },
  computed: {
    cartTotalLength() {
      let totalLength = 0

      for (let i = 0; i < this.cart.items.length; i++) {
        totalLength += this.cart.items[i].quantity
      }

      return totalLength
    }
  }
}
</script>
