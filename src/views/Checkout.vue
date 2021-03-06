<template> 
  <div class="">
    <h1>Checkout</h1>

    <div>
      <table class="">
        <thead>
          <tr>
            <th>Product</th>
            <th>Price</th>
            <th>Quantity</th>
            <th>Total</th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="item in cart.items"
            :key="item.product.card"
            >
            <td>{{ item.product.name }}</td>
            <td>${{ item.product.price }}</td>
            <td>{{ item.quantity }}</td>
            <td>${{ getItemTotal(item).toFixed(2) }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="">
      <h2>Shipping details</h2>
      <p>* All fields are required</p>

      <div class="">
        <div class="">
          <label>First name*</label>
          <input type="text" class="input" v-model="first_name">
        </div>

        <div class="">
          <label>Last name*</label>
          <input type="text" class="input" v-model="last_name">
        </div>

        <div class="">
          <label>E-mail*</label>
          <input type="email" class="input" v-model="email">
        </div>

        <div class="">
          <label>Phone*</label>
          <input type="text" class="input" v-model="phone">
        </div>
        <div class="">
          <label>Address*</label>
          <input type="text" class="input" v-model="address">
        </div>

        <div class="">
          <label>Zip code*</label>
          <input type="text" class="input" v-model="zipcode">
        </div>

        <div class="">
          <label>Place*</label>
          <input type="text" class="input" v-model="place">
        </div>
      </div>

      <div class="" v-if="errors.length">
        <p v-for="error in errors" :key="error">
          {{ error }}
        </p>
      </div>

      <hr>

      <div id="card-element"></div>

      <div id="">
        <template v-if="cartTotalLength">
          <hr>
          <button class="" @click="submitForm">
            Pay with Stripe
          </button>
        </template>
      </div>
    </div>

  </div>
</template>

<script> 
import axios from 'axios'

export default {
  name: 'Checkout',
  data() {
    return {
      cart: {
        items: []
      },
      stripe: {},
      card: {},
      first_name: '',
      last_name: '',
      email: '',
      phone: '',
      address: '',
      zipcode: '',
      place: '',
      errors: []
    }
  },
  mounted() {
    document.title = 'Checkout | Djackets'
    this.cart = this.$store.state.cart

    if (this.cartTotalLength > 0) {
      this.stripe = Stripe('pk_test_51KIixQHb298N4L0QNtIUoUxgn3aU33FC0c17jxHAm88p1rggDtHuhFp06OI2elg3Xg6sCGq0ZjdFqYyD21TzkNu900GOjFHNbC')
      const elements = this.stripe.elements();
      this.card = elements.create('card', { hidePostalCode: true })
      this.card.mount('#card-element')
    }

  },
  methods: {
    getItemTotal(item) {
      return item.quantity * item.product.price
    },
    submitForm() {
      this.errors = []
      if (this.first_name === '') {
        this.errors.push('The first name field is missing!')
      }
      if (this.last_name === '') {
        this.errors.push('The last name field is missing!')
      }
      if (this.email === '') {
        this.errors.push('The email field is missing!')
      }
      if (this.phone === '') {
        this.errors.push('The phone field is missing!')
      }
      if (this.address === '') {
        this.errors.push('The address field is missing!')
      }
      if (this.zipcode === '') {
        this.errors.push('The zip code field is missing!')
      }
      if (this.place === '') {
        this.errors.push('The place field is missing!')
      }
      if (!this.errors.length) {
        this.$store.commit('setIsLoading', true)
        this.stripe.createToken(this.card).then(result => {                    
          if (result.error) {
            this.$store.commit('setIsLoading', false)
            this.errors.push('Something went wrong with Stripe. Please try again')
            console.log(result.error.message)
          } else {
            this.stripeTokenHandler(result.token)
          }
        })
      }
    },
    async stripeTokenHandler(token) {
      const items = []
      for (let i = 0; i < this.cart.items.length; i++) {
        const item = this.cart.items[i]
        const obj = {
          product: item.product.id,
          quantity: item.quantity,
          price: item.product.price * item.quantity
        }
        items.push(obj)
      }
      const data = {
        'first_name': this.first_name,
        'last_name': this.last_name,
        'email': this.email,
        'address': this.address,
        'zipcode': this.zipcode,
        'place': this.place,
        'phone': this.phone,
        'items': items,
        'stripe_token': token.id
      }
      await axios
        .post('/api/v1/checkout/', data)
        .then(response => {
            this.$store.commit('clearCart')
            this.$router.push('/cart/success')
        })
        .catch(error => {
            this.errors.push('Something went wrong. Please try again')
            console.log(error)
        })
        this.$store.commit('setIsLoading', false)
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
