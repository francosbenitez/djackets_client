<template>
  <div class="home">
    Home

    <div class="" v-for="product in latestProducts" :key="product.id">
       <div class="">
        <figure>
          <img :src="product.get_thumbnail">
        </figure>

        <h3>{{ product.name }}</h3>
        <p class="">${{ product.price }}</p>

        View details
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Home',
  data() {
    return {
      latestProducts: []
    }
  },
  mounted() {
    this.getLatestProducts()
  },
  methods: {
    getLatestProducts() {
      axios
        .get('/api/v1/latest-products/')
        .then(response => {
          this.latestProducts = response.data
        }).catch(error => {
          console.log(error)
        })
    }
  }
}
</script>
