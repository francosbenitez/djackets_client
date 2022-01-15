<template>
  <div class="">
    <h2>{{ category.name }}</h2>
    <ProductBox 
      v-for="product in category.products" 
      :key="product.id"
      :product="product"
    />
  </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox'

export default {
  name: 'Category',
  components: {
    ProductBox
  },
  data() {
    return {
      category: {
        products: []
      }
    }
  },
  mounted() {
    this.getCategory()
  },

  // To observe changes in routes and refresh the component.
  // Otherwise, the data will be the same both for the Winter category and
  // the Summer category
  watch: {
    $route(to, from) {
      if (to.name === 'Category') {
        this.getCategory()
      }
    }
  },
  methods: {
    async getCategory() {
      const categorySlug = this.$route.params.category_slug
    
      this.$store.commit('setIsLoading', true)

      await axios
        .get(`/api/v1/products/${categorySlug}`)
        .then(response => {
          this.category = response.data
          document.title = this.category.name + ' | Djackets'

        }).catch(error => {
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)

    }
  }
}
</script>
