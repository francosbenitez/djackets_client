<template>
  <div class="">
    <figure>
      <img :src="product.get_image">
    </figure>

    <h1>{{ product.name }}</h1>

    <p>{{ product.description }}</p>

    <div class="">
      <h2>Information</h2>

      <p>
        <strong>Price:</strong>${{ product.price }}
      </p>

       <div class="">
         <div class="">
          <input 
            type="number" 
            v-model="quantity"
            min="1"
          >
        </div>

        <div class="">
          <button class="" @click="addToCart">Add to cart</button>
        </div>

       </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Product',
  data() {
    return {
      product: {},
      quantity: 1,
    }
  },
  mounted() {
    this.getProduct()
  },
  methods: {
    // To make sure that we don't call 'false' before it's finished,
    // we add 'async'
    async getProduct() {
      this.$store.commit('setIsLoading', true)

      const category_slug = this.$route.params.category_slug
      const product_slug = this.$route.params.product_slug

      await axios
        .get(`/api/v1/products/${category_slug}/${product_slug}`)
        .then(response => {
          this.product = response.data

          document.title = this.product.name + ' | Djackets'
        })
        .catch(error => {
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)
    },
    addToCart() {
      if (isNaN(this.quantity) || this.quantity < 1) {
        this.quantity = 1
      }

      const item = {
        product: this.product,
        quantity: this.quantity
      }

      // We call the addToCart() function inside the mutations
      this.$store.commit('addToCart', item)
    }
  }
}
</script>
