<template>
  <div class="home">
    Home
    <ProductBox 
      v-for="product in latestProducts" 
      :key="product.id"
      :product="product"
    />
  </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox'

export default {
  name: 'Home',
  components: {
    ProductBox
  },
  data() {
    return {
      latestProducts: []
    }
  },
  mounted() {
    this.getLatestProducts()
    document.title = 'Home | Djackets'
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true)

      await axios
        .get('/api/v1/latest-products/')
        .then(response => {
          this.latestProducts = response.data
        }).catch(error => {
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>
