<template> 
  <div class="">
    <div class="">
      <h1>My Account</h1>
    </div>

    <div class="">
      <button @click="logout">
        Log out
      </button>
    </div>

    <div class="">
      <h2>My orders</h2>

      <OrderSummary
        v-for="order in orders"
        :key="order.id"
        :order="order" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import OrderSummary from '@/components/OrderSummary.vue'

export default {
  name: 'MyAccount',
  components: {
    OrderSummary
  },
  data() {
    return {
      orders: []
    }
  },
  mounted() {
    document.title = 'My account | Djackets'
    this.getMyOrders()
  },
  methods: {
    logout() {
      axios.defaults.headers.common["Authorization"]

      localStorage.removeItem("token")
      localStorage.removeItem("username")
      localStorage.removeItem("userid")

      this.$store.commit('removeToken')
      this.$router.push('/')
    },
    async getMyOrders() {
      this.$store.commit('setIsLoading', true)

      await axios
        .get('/api/v1/orders')
        .then(response => {
          this.orders = response.data
        })
        .catch(error => {
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>
