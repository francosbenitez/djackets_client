<template>
  <div>
    <h1>Sign Up</h1>

    <form @submit.prevent="submitForm">
      <div class="">
        <label>Username</label>
        <input type="text" v-model="username">
      </div>

      <div class="">
        <label>Password</label>
        <input type="text" v-model="password">
      </div>

      <div class="">
        <label>Repeat password</label>
        <input type="text" v-model="password2">
      </div>

      <div class="" v-if="errors.length">
        <p v-for="error in errors" :key="error">
          {{ error }}
        </p>
      </div>

      <div class="">
        <button>Sign Up</button>
      </div>

      <hr>

      Or 
      <router-link to="/log-in">
        click here
      </router-link>
      to log in!
    </form>

  </div> 
</template>

<script>
import axios from 'axios'

export default {
  name: 'SignUp',
  data() {
    return {
      username: '',
      password: '',
      password2: '',
      errors: []
    }
  },
  methods: {
    submitForm() {
      this.errors = []

      if (this.username === '') {
        this.errors.push('The username is missing')
      }

      if (this.password === '') {
        this.errors.push('The password is too short')
      }

      if (this.password !== this.password2) {
        this.errors.push('The passwords doesn\'t match')
      }

      if (!this.errors.length) {
        const formData = {
          username: this.username,
          password: this.password
        }

        axios
          .post("/api/v1/users/", formData)
          .then(response => {
            this.$router.push('/log-in')
          })
          .catch(error => {
            if (error.response) {
              for (const property in error.response.data) {
                this.errors.push(`${property}: ${error.response.data[property]}`)
              }
              console.log(JSON.stringify(error.response.data))
            } else if (error.message) {
              this.errors.push('Something went wrong. Please try again')
                
              console.log(JSON.stringify(error))
            }
          })
      }
    }
  }
}
</script>
