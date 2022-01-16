<template>
  <table class="" v-if="cartTotalLength">
    <thead>
      <tr>
        <th>Product</th>
        <th>Price</th>
        <th>Quantity</th>
        <th>Total</th>
        <th></th>
      </tr>
    </thead>

    <tbody>
      <CartItem 
        v-for="item in cart.items"
        :key="item.product.id"
        :initialItem="item"
        @removeFromCart="removeFromCart" />
    </tbody>
  </table> 
  <p v-else>You don't have any products in your cart...</p>

  <div class="">
    <h2>Summary</h2>
    <strong>$ {{ cartTotalPrice.toFixed(2) }}</strong>,
    {{ cartTotalLength }} items

    <hr>

    <router-link to="/cart/checkout">
      Proceed to checkout
    </router-link>
  </div>
</template>

<script>
import axios from 'axios'
import CartItem from '@/components/CartItem.vue'

export default {
  name: 'Cart',
  components: {
    CartItem
  },
  data() {
    return {
      cart: {
        items: []
      }
    }
  },
  mounted() {
    this.cart = this.$store.state.cart
  },
  methods: {
    removeFromCart(item) {
      console.log('item', item)
      this.cart.items = this.cart.items.filter(i => i.product.id !== item.product.id)
    }
  },
  computed: {
    cartTotalLength() {
      return this.cart.items.reduce((acc, curVal) => {
        return acc += curVal.quantity
      }, 0)
    },
    cartTotalPrice() {
      return this.cart.items.reduce((acc, curVal) => {
        return acc += curVal.product.price * curVal.quantity
      }, 0)
    }
  }
}
</script>
