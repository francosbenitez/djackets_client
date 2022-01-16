<template>
  <div id="wrapper">

    <form method="get" action="/search">
      <input type="text" name="query">
    </form>

    <router-link to="/">Djackets</router-link>
    <router-link to="/summer">Summer</router-link>
    <router-link to="/winter">Winter</router-link>
    
    <div class="buttons">

      <template v-if="$store.state.isAuthenticated">
        <router-link to="/my-account">My account</router-link>
      </template>

      <template v-else>
        <router-link to="/log-in">Log in</router-link>
      </template>

      <router-link to="/cart">
        <span class="icon"><i class="fas fa-shopping-cart"></i></span>
        <span>
          Cart
          ({{ cartTotalLength }})
        </span>
      </router-link>
    </div>

    <div 
      class="is-loading-bar" 
      :class="{'is-loading': $store.state.isLoading }"
      >
      <!-- Loading... -->
      <div class="lds-dual-ring"></div>
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
import axios from 'axios'

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

    const token = this.$store.state.token

    if (token) {
      axios.defaults.headers.common['Authorization'] = "Token " + token
    } else {
      axios.defaults.headers.common['Authorization'] = ""
    }
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

<style lang="scss">
.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}

.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #ccc;
  border-color: #ccc transparent #ccc transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}

@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.is-loading-bar {
  height: 0;
  overflow: hidden;
  -webkit-transition: all 0.3s;
  transition: all 0.3s;
  &.is-loading {
    height: 80px;
  }
}
</style>
